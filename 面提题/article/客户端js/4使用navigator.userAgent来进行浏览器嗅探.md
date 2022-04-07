# 使用navigator.userAgent来进行浏览器嗅探

>Window对象的navigator属性引用的是包含浏览器厂商和版本信息的Navigator对象。

> 在需要解决存在于某个特定的浏览器的特定版本中的特殊的bug时，浏览器嗅探仍然有价值。

~~~js
/*--------------------------------------------------------------------------------*
 2  * 功能描述：使用navigator.userAgent来进行浏览器嗅探
 3  * 原理：为客户端嗅探定义browser.name和browser.version，这里使用了jQuery 1.4.1中的代码
 4  *          name和number都是字符串，对于不同的浏览器输出结果不一样
 5  *        "webkit":Safari或Chrome;版本号Webkit的版本号
 6  *         "opera":Opera;版本号就是浏览器的版本号
 7  *         "mozilla": Firefox或者其他基于gecko内核的浏览器;版本号是Gecko的版本号
 8  *         "msie":IE;版本号就是浏览器的版本号
 9  
11  *--------------------------------------------------------------------------------*/

var browser = function(){
    var s = navigator.userAgent.toLowerCase();
    var match = /(webkit)[ \/]([\w.]+)/.exec(s) ||
         /(opera)(?:.*version)?[ \/]([\w.]+)/.exec(s) ||
         /(msie)([\w.]+)/.exec(s) ||
         !/compatible/.test(s) && /(mozilla)(?:.*? rv:([\w.]+))?/.exec(s) ||
         [];
    return {
        name:match[1] || "",
        version:match[2] || "0"
    }     
}
~~~