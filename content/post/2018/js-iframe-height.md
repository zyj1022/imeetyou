---
title: Js动态获取iframe子页面的高度总结
date:  2018-03-06T13:32:08+08:00
tags: ["frontend","iframe"]
categories: ["frontend"]
---

## 问题
某个页面引用的是个动态高度的iframe，不希望iframe出现滚动条，只希望使用浏览器滚动条，这个时候我们就需要动态改变iframe的高度。
## 方法1：父级页面获取子级页面的高度 给元素设置高度
这方法是用在父级页面里的，通过获取子级页面的高度给iframe设置高度

涉及了一些兼容问题:

IE用attachEvent | 3C用onload来判断子页面是否加载完成。

IE用contentWindow | 3C用contentDocument来获取子页面

IE用document.documentElement.scrollHeight（兼容ie6 ie7）| 3C用body.scrollHeight获取页面高度

```
function setIframeHeight(id){
    try{
        var iframe = document.getElementById(id);
        if(iframe.attachEvent){
            iframe.attachEvent("onload", function(){
                iframe.height =  iframe.contentWindow.document.documentElement.scrollHeight;
            });
            return;
        }else{
            iframe.onload = function(){
                iframe.height = iframe.contentDocument.body.scrollHeight;
            };
            return;                 
        }     
    }catch(e){
        throw new Error('setIframeHeight Error');
    }
}
```

## 方法2：子级页面给父级页面元素设置高度
这方法是用在子级页面里的，通过获取子级页面的高度给父级iframe设置高度

子级页面通过parent获取父级iframe 给iframe设置高度，兼容同方法1。

缺点是刷父级页面时iframe有缓存，需求清理缓存才能生效。

```
function setParentIframeHeight(id){
    try{
        var parentIframe = parent.document.getElementById(id);
         if(window.attachEvent){
            window.attachEvent("onload", function(){
                parentIframe.height = document.documentElement.scrollHeight;
            });
            return;
        }else{
            window.onload = function(){
                parentIframe.height = document.body.scrollHeight;
            };
            return;                 
        }     
    }catch(e){
        throw new Error('setParentIframeHeight Error');
    }
}
```

### 需要注意的跨域操作
如果两个页面有一种情况，两个子域名：

aaa.xxx.com
bbb.xxx.com

需要将两个页面都设置如：

document.domain ="abc.com.cn";

这样这两个页面就可以互相操作了。也就是实现了同一基础域名之间的"跨域"。

利用document.domain实现跨域：
前提条件：这两个域名必须属于同一个基础域名!而且所用的协议，端口都要一致，否则无法利用document.domain进行跨域

Javascript出于对安全性的考虑，而禁止两个或者多个不同域的页面进行互相操作。
相同域的页面在相互操作的时候不会有任何问题。
