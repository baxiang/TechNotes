
概念: BOM(Browser Object Model) 是指浏览器对象模型,浏览器对象模型提供了独立于内容的、可以与浏览器窗口进行互动的对象结构。BOM由多个对象组成，其中代表浏览器窗口的Window对象是BOM的顶层对象，其他对象都是该对象的子对象。

通俗理解:  把浏览器当做对象,通过访问对象的属性,实现操作浏览器的一组方法
2. 页面加载事件

2.1 load事件

    window.onload = function () {
      
      // 当页面加载完所有内容（包括图像、脚本文件、CSS 文件等）执行
    }

2.2 unload事件

    window.onunload = function () {
      // 当用户退出页面时执行(关闭页面)
    }


小结:
window.onload事件 是页面所有资源加载完成时触发
window.onunload事件 是用户退出页面时触发
