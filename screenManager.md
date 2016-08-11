ScreenManager
-------------


######基本模块
```javascript
var ScreenClass = (function(){
    function ScreenClass(){
    }
    ScreenClass.prototype ={
        show:function(){
        },
        close:function(){
        }
    };
    return ScreenClass;
})();
```

######CORE
```javascript
ScreenManager.applyScreen = function () {
var screenClass = arguments[0];
if (!screenClass) {
    alert('Can\'t find the page.');
    return;
}
if (ScreenCurrent) {
    window.onresize = null;
    // 如果 close 方法返回 false，则直接返回，不进行新页面的渲染
    if (ScreenCurrent.close && ScreenCurrent.close() === false) {
        return;
    }
}
var screenObj = Object.create(screenClass.prototype);
ScreenCurrent = (screenClass.apply(screenObj, Array.prototype.slice.call(arguments, 1)) || screenObj);
if (ScreenCurrent.onresize) window.onresize = ScreenCurrent.onresize;
    ScreenCurrent.show();
};
```


######hash解析
```javascript
/***
 * 获取URL中参数,返回参数的map类型
 * @returns {{}}
 * @private
 */
ScreenManager._getHashParamsMap = function () {
    var list, map = {};
    list = location.hash.substr(1).split('&');
    for (var m = 0; m < list.length; m++) {
        var item = list[m].split('=');
        map[item[0]] = item[1];
    }
    return map;
}
```

######screen类解析
用于screen类的参数注入
```javascript
//获取函数的参数列表
ScreenManager._getFunctionParams = function (func) {
    var STRIP_COMMENTS = /((\/\/.*$)|(\/\*[\s\S]*?\*\/))/mg,//去除参数中的注释
        ARGUMENT_NAMES = /([^\s,]+)/g, fnStr, result; //提取参数列表

    fnStr = func.toString().replace(STRIP_COMMENTS, '');
    result = fnStr.slice(fnStr.indexOf('(') + 1, fnStr.indexOf(')')).match(ARGUMENT_NAMES);
    if (result === null) {
        result = [];
    }
    return result;
};  
```

######根据hash切换不同的页面
通过注册hashchange事件,自动切换screen. 

hash中的参数中

page为screenClass,其他参数通过参数注入的方式注入到screenClass中实现screenClass的初始化, 接着调用screen对象的show方法.



######权限拦截
在页面切换前进行权限拦截

######相关
每个函数内部都有个arguments变量
argument是类数组,可以通过Array.prototype.slice处理转为真正的数组

[apply函数](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)

[call函数](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call)

[apply和call的区别](http://stackoverflow.com/questions/1986896/what-is-the-difference-between-call-and-apply)


