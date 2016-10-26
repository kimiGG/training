##factory btn控件代码架构学习心得
>　　factory控件的基类为Widge,控件分为html控件和canvas控件(canvas的显示更加多样化,而html更容易通过DOM操作来控制),因为canvas控件的共同的特性较少所以只继承了Widge类,而htm控件继承了htmlWidget和widge两个类.同时继承这两个类使用core.js的Mixin方法.比如Mixin({a:1},{a:2,b:2},{c:3}),返回(a:2,b:2,c:3).core.js是factory的核心代码.
>  　　btn控件代码在htmlButton.js中,其中_format方法是为了兼容老数据,比如没有pageId就赋值为空.还有show()方法是每个控件必须有的方法.show方法中出现的HTMLParser将html代码转换成dom对象(html代码必须包含一个根节点).还有一个fixZoom方法,是针对缩放所做的处理,在谷歌浏览器中最小字体是12px,解决缩放只有字体小于12px的问题.在执行完show方法后,按钮就显示在页面上.
   ```
   /** override */
    HtmlButton.prototype.show = function () {
	　　var model = this.store.model;
	 　　SuperClass.prototype.show.apply(this, arguments);
	 　　this.shape = HTMLParser(this.tpl);
	 　　this.shape.id = model._id();
	 　　this.shape.style.position = 'absolute';
	 　　this.update();
	 　　this.layer.add(this.shape);
	 　　this.fixZoom();
  };
  ```
   
>　　update()方法也是每个控件都有的,可以根据参数判断是x,y,亦或者是width,height,颜色,自定义样式等发生了变化.update()方法都可以检测出来,然后做相应处理.控件里的方法都是重写的基类的方法。
>　　这个控件的功能比较齐全,可以调整按钮的样式,位置,长宽,使用起来也比较方便,命名也方便维护比如Widge类写在Widge.js里面.类名和文件名一致,不仅方便维护也让其他开发者阅读起来更轻松.各个控件风格统一.也是有利于阅读维护.
> 　　其中,我也发现了一些我觉得可以改善的地方:
>  
 * 按钮控件![Alt text](./1477458217152.png)在向上下左右拉动的时候不要显示默认的箭头. ![Alt text](./1477458331158.png)比如像右上方拖动的时候改为如图所示的箭头..类似的还有拖动的时候![Alt text](./1477458609998.png)
 用cursor:move比较好.
 * ![Alt text](./1477458619963.png)跳转的时候下拉菜单为灰色,容易让人以为是不可用的.
 *  刷新之后回到的是facotory首页,而不是pageScreen的页面.刷新之后回到刚刚操作的页面会好一些.
 * 操作时选中了一个按钮,按住ctrl和alt进行缩放的时候就没有再选中按钮了.在正常的页面选中的后缩放还是选中的.

