浏览器原理
-------------

http://kb.cnblogs.com/page/129756/

http://www.html5rocks.com/zh/tutorials/internals/howbrowserswork/

######DOM
 

DOM对象是浏览器解析HTML脚本生成的，最终输出一个树状结构的对象。

######CSSOM

CSSOM对象是浏览器解析CSS脚本生成的，最终也是输出类似DOM的树状结构。

######RenderTree


DOM 与 CSSOM 融合成一棵RenderTree（渲染树），随后计算每个可见元素的布局，并输出给绘制过程，在屏幕上渲染像素。优化这里的每一步对实现最佳渲染性能至关重要。

并不是DOM中所有元素都会出现在RenderTree中，比如head，meta,script等标签，伪类，伪元素，又或者是结合CSS属性，比如display:none的节点及其子节点都不会在渲染树中出现。


######布局

 

有了渲染树，就进入布局阶段。根据渲染树种确定的每个DOM元素的样式规则，浏览器就能具体计算每个DOM元素最终在屏幕上显示的大小位置，宽高等等几何属性。由于文档流中的布局是相对的，因此每个元素的布局发生变化，会联动引发其他元素的布局变化。

 

######绘制

 

绘制就是在已确定了几何属性的元素上填充像素，比如绘制文字，颜色，图像，边框，阴影等等可视元素。这期间会产生多个图层

 

######合并渲染层

来到这里，浏览器的渲染过程就接近尾声。每个图层绘制完，浏览器会将其按照合理的顺序合并到同一图层，并显示在浏览器上。

![render](https://github.com/beop/training/blob/master/image/renderTree.jpg?raw=true)
 

https://developers.google.com/web/fundamentals/performance/critical-rendering-path/render-tree-construction?hl=zh-cn

https://github.com/zhangyaowu/CN-Chrome-DevTools/blob/master/md/Performance-Profiling/Performance-profiling-with-the-Timeline.md

 

 

调试技巧
-------------

* 常见工具使用

* Debugger

* DOM断点

* 条件断点

* 事件监听器断点

* Source修改版本回退右键打开Local Modifications

* Snippet 比如html to script

```javascript
(function() {

  var url = location;

  var querystring = location.search.slice(1);

  var tab = querystring.split("&").map(function(qs) {

    return { "Key": qs.split("=")[0], "Value": qs.split("=")[1], "Pretty Value": decodeURIComponent(qs.split("=")[1]).replace(/\+/g," ") }

  });

 

  console.group("Querystring Values");

  console.log("URL: "+url+"\nQS:  "+querystring);

  console.table(tab);

  console.groupEnd("Querystring Values");

 

})();
```

* $0

* Async调试

* iframe 调试 切换

* 常用快捷键

    Ctrl + Shift + J:进入console面板
    
    ctrl+p 项目中定位文件
    
    ctrl+shif+o 文件中定位成员函数
    
    ctrl+g 行数
    
    Ctrl +Shift+E 被选中代码在控制台中打印出console信息
    
    Ctrl + F: 搜索内容
    
    Ctrl + Shift + F: 在所有代码中搜索


https://segmentfault.com/a/1190000000481884