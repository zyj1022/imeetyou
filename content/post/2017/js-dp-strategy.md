---
title: 设计模式之策略模式(Strategy)
date: 2017-04-01 12:53:39
categories:
- frontend
tags:
- 设计模式
- Strategy
- js
---

在程序设计中，我们也常常遇到这样的情况，要实现某一个功能有多种方案可以选择。比如一个压缩文件的程序，既可以选择 zip 算法，也可以选择 gzip 算法。
这些算法灵活多样，而且可以随意互相替换。这种解决方案就是将要介绍的策略模式。

## 定义

策略模式的定义是:定义一系列的算法，把它们一个个封装起来，并且使它们可以相互替换。

从定义上看，策略模式就是用来封装算法的。但如果把策略模式仅仅用来封装算法，未免有一点大材小用。在实际开发中，我们通常会把算法的含义扩散开来，使策略模式也可以用来封装一系列的“业务规则”。只要这些业务规则指向的目标一致，并且可以被替换使用，我们就可以用策略模式来封装它们。

<!--more-->

## 使用实例

### 表单验证

在一个 Web 项目中，注册、登录、修改用户信息等功能的实现都离不开提交表单。

在将用户输入的数据交给后台之前，常常要做一些客户端力所能及的校验工作，比如注册的时候需要校验是否填写了用户名，密码的长度是否符合规定等等。这样可以避免因为提交不合法数据而带来的不必要网络开销。

假设我们正在编写一个注册的页面，在点击注册按钮之前，有如下几条校验逻辑。

* 用户名不能为空。
* 密码长度不能少于 6 位
* 手机号码必须符合格式。


在理解策略模式之前，通常我们遇到类似多条件的业务，按照swith语句来判断，但是这就带来几个问题，首先如果增加需求的话，我们还要再次修改这段代码以增加逻辑，而且在进行单元测试的时候也会越来越复杂，代码如下：

```
 var validator = {
     validate: function (value, type) {
         switch (type) {
             case 'isNonEmpty ':
                     return true; // NonEmpty 验证结果
             case 'minLength ':
                     return true; // minLength 验证结果
                     break;
             case 'isMobile ':
                     return true; // isMobile 验证结果
             default:
                     return true;
         }
     }
 };
 // 测试
 alert(validator.validate("123", "isNonEmpty"));
```

### 用策略模式重构表单校验

用策略模式来重构表单校验的代码，我们可以将相同的工作代码单独封装成不同的类，然后通过统一的策略处理类来处理，OK，我们先来定义策略处理类，代码如下：

```
var validator = {

    // 所有可以的验证规则处理类存放的地方，后面会单独定义
    types: {},

    // 验证类型所对应的错误消息
    messages: [],

    // 当然需要使用的验证类型
    config: {},

    // 暴露的公开验证方法
    // 传入的参数是 key => value对
    validate: function (data) {

        var i, msg, type, checker, result_ok;

        // 清空所有的错误信息
        this.messages = [];

        for (i in data) {
            if (data.hasOwnProperty(i)) {

                type = this.config[i];  // 根据key查询是否有存在的验证规则
                checker = this.types[type]; // 获取验证规则的验证类

                if (!type) {
                    continue; // 如果验证规则不存在，则不处理
                }
                if (!checker) { // 如果验证规则类不存在，抛出异常
                    throw {
                        name: "ValidationError",
                        message: "No handler to validate type " + type
                    };
                }

                result_ok = checker.validate(data[i]); // 使用查到到的单个验证类进行验证
                if (!result_ok) {
                    msg = "Invalid value for *" + i + "*, " + checker.instructions;
                    this.messages.push(msg);
                }
            }
        }
        return this.hasErrors();
    },

    // helper
    hasErrors: function () {
        return this.messages.length !== 0;
    }
};
```

然后剩下的工作，就是定义types里存放的各种验证类了：

```
// 验证给定的值是否不为空
validator.types.isNonEmpty = {
    validate: function (value) {
        return value !== "";
    },
    instructions: "传入的值不能为空"
};

// 验证给定的值是否是数字
validator.types.isNumber = {
    validate: function (value) {
        return !isNaN(value);
    },
    instructions: "传入的值只能是合法的数字，例如：1, 3.14 or 2010"
};

// 验证给定的值是否只是字母或数字
validator.types.isAlphaNum = {
    validate: function (value) {
        return !/[^a-z0-9]/i.test(value);
    },
    instructions: "传入的值只能保护字母和数字，不能包含特殊字符"
};
```

使用的时候，我们首先要定义需要验证的数据集合，然后还需要定义每种数据需要验证的规则类型，代码如下：

```
var data = {
    first_name: "Tom",
    last_name: "Xu",
    age: "unknown",
    username: "TomXu"
};

validator.config = {
    first_name: 'isNonEmpty',
    age: 'isNumber',
    username: 'isAlphaNum'
};
```

最后，获取验证结果的代码就简单了：

```
validator.validate(data);

if (validator.hasErrors()) {
    console.log(validator.messages.join("\n"));
}
```

## 策略模式的优缺点

策略模式是一种常用且有效的设计模式，我们可以总结出策略模式的一些优点：

1. 策略模式利用组合、委托和多态等技术和思想，可以有效地避免多重条件选择语句。
2. 策略模式提供了对开放—封闭原则的完美支持，将算法封装在独立的 strategy 中，使得它们易于切换，易于理解，易于扩展。
3. 策略模式中的算法也可以复用在系统的其他地方，从而避免许多重复的复制粘贴工作。
4. 在策略模式中利用组合和委托来让 Context 拥有执行算法的能力，这也是继承的一种更轻便的替代方案。

当然，策略模式也有一些缺点，但这些缺点并不严重。

首先，使用策略模式会在程序中增加许多策略类或者策略对象，但实际上这比把它们负责的 逻辑堆砌在 Context 中要好。

其次，要使用策略模式，必须了解所有的 strategy，必须了解各个 strategy 之间的不同点， 这样才能选择一个合适的 strategy。比如，我们要选择一种合适的旅游出行路线，必须先了解选 择飞机、火车、自行车等方案的细节。此时 strategy 要向客户暴露它的所有实现，这是违反最少 知识原则的。

## 总结

策略模式定义了一系列算法，从概念上来说，所有的这些算法都是做相同的事情，只是实现不同，他可以以相同的方式调用所有的方法，减少了各种算法类与使用算法类之间的耦合。

从另外一个层面上来说，单独定义算法类，也方便了单元测试，因为可以通过自己的算法进行单独测试。

实践中，不仅可以封装算法，也可以用来封装几乎任何类型的规则，是要在分析过程中需要在不同时间应用不同的业务规则，就可以考虑是要策略模式来处理各种变化。


-----

> 参考引用资料
>
> [汤姆大叔的博客——深入理解JavaScript系列](http://www.cnblogs.com/TomXu/archive/2011/12/15/2288411.html)
>

-----

> 参考引用资料
>
> 《JavaScript设计模式与开发实践》
>
> [汤姆大叔的博客——深入理解JavaScript系列](http://www.cnblogs.com/TomXu/archive/2011/12/15/2288411.html)
>
