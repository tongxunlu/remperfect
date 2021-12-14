# remperfect
完美的rem解决方案,将 html root的像素改为 vw,
css 基础代码  reset.css
```css
html{ font-size:1px }
#
防止页面样式因为脚本没执行完有闪现

#
```javascript
"use strict";
+function(){
  var dom = [];
  dom.isReady = false;
  dom.isFunction = function(obj){
    return Object.prototype.toString.call(obj) === "[object Function]";
  }
  dom.Ready = function(fn){
    dom.initReady();//如果没有建成DOM树，则走第二步，存储起来一起杀
    if(dom.isFunction(fn)){
      if(dom.isReady){
        fn();//如果已经建成DOM，则来一个杀一个
      }else{
        dom.push(fn);//存储加载事件
      }
    }
  }
  dom.fireReady =function(){
    if (dom.isReady)  return;
    dom.isReady = true;
    for(var i=0,n=dom.length;i<n;i++){
      var fn = dom[i];
      fn();
    }
    dom.length = 0;//清空事件
  }
  dom.removeReady=function(){
    document.removeEventListener( "DOMContentLoaded", dom.removeReady, false );//清除加载函数
    dom.fireReady();
  };
  dom.initReady = function(){
    if (document.addEventListener) {
      document.addEventListener( "DOMContentLoaded",dom.removeReady , false );
    }else{
      if (document.getElementById) {
        document.write("<script id=\"ie-domReady\" defer='defer'src=\"//:\"><\/script>");
        document.getElementById("ie-domReady").onreadystatechange = function() {
          if (this.readyState === "complete") {
            dom.fireReady();
            this.onreadystatechange = null;
            this.parentNode.removeChild(this)
          }
        };
      }
    }
  };
  window.DOM2=dom;
}(window);

function htmlSize() {
    var NATIVE_W = 750, NATIVE_H = 1334; //390 752
    var root = document.getElementsByTagName('html')[0];
	var cw;
    var w =  document.documentElement.clientWidth;
	var	h = document.documentElement.clientHeight;

	//传统px像素模式
	// if ((w / h) > (NATIVE_W / NATIVE_H)) {
	// 	cw = h / (NATIVE_H / 100)
	// } else {
	// 	cw = w / (NATIVE_W / 100)
	// }
    // root.style.fontSize=cw+"px";
    // root.className="remShow";

  //  vw计算模式 2021-12-13 测试TODO 待验证
  // 1/(375/100)/2,页面原来是15px的 css写15rem即可
  //  1/(375/100)/2==0.133vw 存到html root 意思是 1px==0.133vw
  // 设计稿 12px==  实际页面css 12 rem
    root.style.fontSize=(1/(w/100))+"vw";
    root.className="remShow";
}

DOM2.Ready(function(){
    htmlSize();
});

window.onresize = function(){
	htmlSize()
}

//# sourceURL=ui\common\rem.js

