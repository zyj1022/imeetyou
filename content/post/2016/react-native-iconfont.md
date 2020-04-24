---
title: 如何在React Native中的使用自定义iconfont
date: 2016-11-15 16:46:11
categories:
- frontend
tags:
- frontend
- react-native
- iconfont
---

# React Native中的iconfont


关于在React Native中使用iconfont，网上已有很多非常好的解决方案，用的最多的就是 [react-native-vector-icons](https://github.com/oblador/react-native-vector-icons), 这个库支持很多常用的iconfont,比如FontAwesome, Ionicons, MaterialIcons等等。

但是这个库依赖了不少iOS和Android的原生代码，这让一个前端开发脸上浮现了一个大大的懵逼。 而且自带的字体文件都偏大，做起精简来简直想哭，更别说加入自定义的iconfont了。

<!-- more -->


# 如何生成自定义的iconfont文件

这里我一般通过 [icomoon](https://icomoon.io/) 来实现,将设计好的图标字体一般为 `.svg` 文件,导入到icomoon 里，

![icomoon](http://og8z552x2.bkt.clouddn.com/icomoon.png)

选择我们需要的字体图标，然后 `Generate Font`, 之后 下载 demo 包 内含我们需要的 fonts 文件


# IconFont的使用原理

其实IconFont就是一些文字，通过在web上的使用，我们可以大概猜出使用方法：

- 1.指定字体集
- 2.把对应的16进制码当成文字写到文本中

在React Native中同样如此，我们可以通过 [react-native-vector-icons](https://github.com/oblador/react-native-vector-icons) 的源代码来验证我们的想法。

打开 `react-native-vector-icons/FontAweson.js` 文件([线上地址](https://github.com/oblador/react-native-vector-icons/blob/master/FontAwesome.js))可以看到一个大大的json对象

```
var createIconSet = require('./lib/create-icon-set');
var glyphMap = {
  "glass": 61440,
  "music": 61441,
  "search": 61442,
  .
  . // 此处省略500+行
  .
};
var FontAwesome = createIconSet(glyphMap, 'FontAwesome', 'FontAwesome.ttf');
module.exports = FontAwesome;
module.exports.glyphMap = glyphMap;
```

看到这些亲切的数字了吗，61440，这不就是传说中的16进制的FXXX的十进制吗？

16进制有了，写到哪里呢，继续看createIconSet方法。

其中的Icon组件的render方法：

```
render: function() {
  var { name, size, color, style, ...props } = this.props;
  var glyph = glyphMap[name] || '?';
  if(typeof glyph === 'number') {
    glyph = String.fromCharCode(glyph);
  }
  size = size || DEFAULT_ICON_SIZE;
  var styleDefaults:Object = {
    fontSize: size,
    fontWeight: 'normal',
    fontStyle: 'normal',
    color,
  };
  props.style = [styleDefaults, style];
  props.ref = (component) => this._root = component;
  styleDefaults.fontFamily = fontReference;
  console.log(this.props.children)
  return (<Text {...props}>{glyph}{this.props.children}</Text>);
}
```


其中，最重要的四句话_(我本来写的是两句话，结果越看越多)_：

`var glyph = glyphMap[name] || '?';` 把刚才的`6xxxx`找到；

`glyph = String.fromCharCode(glyph); `转成Unicode编码字符串；

`styleDefaults.fontFamily = fontReference;`指定字符集；

`return (<Text {...props}>{glyph}{this.props.children}</Text>);` 把Unicode字符写到`Text`组件中。

基本和我们的猜想一样，哇哈哈哈。

# Font的基本知识

由上可知，我们主要需要这个Icon所对应的Unicode码，那这个Unicode码又是神马呢？

实际上，一个字体通常由数个表(table)构成，字体的信息存储在表中。一个最基本的字体文件一定会包含以下表：

* cmap: Char­ac­ter to glyph map­ping
* head: Font header
* hhea: Hor­i­zon­tal header
* hmtx: Hor­i­zon­tal met­rics
* maxp: Max­i­mum pro­file
* name: Nam­ing table
* OS/​2: OS/​2 and Win­dows spe­cific met­rics
* post: Post­Script in­for­ma­tion

而使用TrueType曲线绘制的字体则会包含如下表：

* cvt: Con­trol Value Table
* fpgm: Font pro­gram
* glyf: Glyph data
* loca: In­dex to lo­ca­tion
* prep: CVT Pro­gram
* gasp: Grid​-​fit­ting/​Scan​-​con­ver­sion (op­tional table)
 
上面列了很多，最重要的其实是第一个表看这高大上的说明Char­ac­ter to glyph map­ping。

如果把字体文件转成类xml格式，这个表类似：

```
<cmap> 
    <cmap_format_4 platformID="3" platEncID="1" language="0">
          <map code="0xf600" name="uniF600"/>
          <map code="0xf601" name="uniF601"/>
          <map code="0xf602" name="uniF602"/>
          <map code="0xf603" name="uniF603"/>
    </cmap_format_4>
</cmap>
```

这里的0xf600不就是我们想要的吗，而后面的name就类似与每个字符的命名。

![font-baidu](http://og8z552x2.bkt.clouddn.com/font-baidu.png)


这里最好给每个icon定一个易于理解的名字，可以使用 [http://font.baidu.com/editor](http://font.baidu.com/editor)



# 如何使用自定义的IconFont

有了上面的摸索，要支持自己的IconFont并不难。只需要把字符对应表给整出来就可以了，代码如下：

```
var map = {"arrow":"62976","checked":"62977","checked-s":"62978","tag-svip":"62995"};
module.exports = (name)=>String.fromCharCode(map[name]);
```

使用的时候：

```
import icon from "./fontConf";
export default class  IconExample extends Component {
    render() {
        return (
            <View style={styles.container}>
                <Text style={{fontFamily: 'FontIconQui',fontSize:30}}>
                    arrow-icon:{icon('arrow')}
                </Text>
                <Text style={{fontFamily: 'FontIconQui',fontSize:30, color:"#ff4444"}}>
                    vip-icon:{icon('tag-svip')}
                </Text>
                <Text style={{fontFamily: 'FontIconQui',fontSize:30, color:"#ff4444"}}>
                    tag-svip:{icon('tag-svip')}
                </Text>
            </View>
        )
    }
}
```

另外，在工程中，需要引入字体文件：

* Android： 把字体文件拷贝到`[project root]/android/app/src/main/assets/fonts/`
* iOS: 把字体文件拖到对应的Xcode工程里面，勾选 `Add to targets` 和 `Create groups`，修改`Info.plist` 文件，添加属性 `Fonts provided by application`，在这个属性下添加相应字体文件名的`item`，如下图：

![font-baidu](http://og8z552x2.bkt.clouddn.com/xcode.png)

iOS上添加字体文件具体的流程可以参考 [https://github.com/oblador/react-native-vector-icons#option-manually](https://github.com/oblador/react-native-vector-icons#option-manually)。 

# 如何提取字符对应表

打开 [百度字体编辑器](http://font.baidu.com/editor/),导入我们的之前的自定义字体文件

![font-baidu](http://og8z552x2.bkt.clouddn.com/baidueditor.png)

如下将上图中的 `E90C`、`E90D` 转换为十进制 `59660`、`59661` 就是我们想要的字符。

![hexconvert.png](http://og8z552x2.bkt.clouddn.com/hexconvert.png)

在线转换进制 [http://tool.oschina.net/hexconvert/](http://tool.oschina.net/hexconvert/)

然后新建 `iconfont.js` 

```
const map = {
    "angle-left": "59648",
    "angle-right": "59649",
    "ok": "59650",
    "alarm": "59651",
    "edit": "59652",
    "eye-close": "59653",
    "eye-open": "59654"
};

module.exports = (name) => String.fromCharCode(map[name]);
module.exports.map = map;

```

在 React-native 中使用的时候非常简单,首先导入

```
import icon from 'iconfont';
```

然后在需要使用的地方,

```
<Text style={styles.icon}>{icon('edit')}</Text>
```
看看是不是在界面中已经出现了你想要的图标，至于大小颜色，反正是字体嘛，用样式去控制咯～。


