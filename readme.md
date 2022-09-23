1.安装 vscode 的插件
easy less

2.cssrem 默认 16px 为 1rem,能将 css=>rem

3.https://github.com/amfe/lib-flexible.git

4.响应式开发
超小屏幕（手机，小于 768px）：设置宽度为 100%
小屏设备（平板，大于等于 768px）：设置宽度为 750px
中等屏幕（桌面显示器，大于等于 992px）：设置宽度为 970px
宽屏设备（大桌面显示器，大于等于 1200px）：设置宽度为 1170px
5.bootstrap

6.移动端 1px 边框
原因：CSS 中的 1px 并不能和移动设备上的 1px 划等号。它们之间的比例关系有一个专门的属性来描述：
// 设备像素比
window.devicePixelRatio = 设备的物理像素 / CSS 像素
方法一: 
       直接写 0.5px
       border:0.5px solid #E5E5E5;
虽然解决问题了，但是实用性不高，首先，得考虑 IOS 系统需要 8 及以上的版本，安卓系统则有不兼容问题
方法二: 
        用图片代替边框
        border: 1px solid transparent;
        border-image: url('xxx.jpg') 2 repeat;
虽然解决问题了，但是后期样式调整会让人奔溃，比如颜色调整得 UI 小伙伴重新上传图片，然后又要修改代码，或者直接文件替换有涉及到图片缓存问题，如果后期来了一个要有边框圆角需求完全没法搞。
方法三:
      background渐变
      background-position: left top;
      background-image: -webkit-gradient(linear,left bottom,left top,color-stop(0.5,transparent),color-stop(0.5,#e0e0e0),to(#e0e0e0));
   代码多，展示的边框实际是在原本的border空间内部的，如果元素背景色有变化的样式, 边框线也会消失；最后也不能适应圆角样式
方法四： 
      box-shadow模拟边框实现
      /* x 偏移量 | y 偏移量 | 阴影模糊半径 | 阴影扩散半径 | 阴影颜色 */
      box-shadow: 0  -1px 1px -1px #e5e5e5,   //上边线
                  1px  0  1px -1px #e5e5e5,   //右边线
                  0  1px  1px -1px #e5e5e5,   //下边线
                  -1px 0  1px -1px #e5e5e5;   //左边线
   展示的阴影和边框一个样，但如果有边框还要有虚影样式就没法搞，鱼和熊掌不可兼得。
方法五：(其次)
      伪元素先放大后缩小
      实现方式：在目标元素的后面追加一个 ::after 伪元素，让这个元素布局为 absolute 之后、整个伸展开铺在目标元素上，然后把它的宽和高都设置为目标元素的两倍，border值设为 1px。接着借助 CSS 动画特效中的放缩能力，把整个伪元素缩小为原来的 50%。此时，伪元素的宽高刚好可以和原有的目标元素对齐，而 border 也缩小为了 1px 的二分之一，间接地实现了 0.5px 的效果。
        .hairline{
          position: relative;
            &::after{
               content: '';
               position: absolute;
               top: 0;
               left: 0;
               height: 1px;
               width: 100%;
               transform: scaleY(0.5);
               transform-origin: 0 0;
               background-color: #EDEDED;
            }
         }
方法六:(最好的方案)
     设置viewport解决问题
     利用viewport+rem+js 实现的，边框1px直接写上自动转换。
     <!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta http-equiv="X-UA-Compatible" content="IE=edge" />
        <meta name="viewport" id="WebViewport" content="initial-scale=1, maximum-scale=1, minimum-scale=1, user-scalable=no" />
        <title>Document</title>
        <style type="text/css"></style>
    </head>
    <body>
        <script type="text/javascript">
            let viewport = document.querySelector('meta[name=viewport]')
            //下面是根据设备像素设置viewport
            if (window.devicePixelRatio == 1) {
                viewport.setAttribute('content', 'width=device-width,initial-scale=1, maximum-scale=1, minimum-scale=1, user-scalable=no')
            }
            if (window.devicePixelRatio == 2) {
                viewport.setAttribute('content', 'width=device-width,initial-scale=0.5, maximum-scale=0.5, minimum-scale=0.5, user-scalable=no')
            }
            if (window.devicePixelRatio == 3) {
                viewport.setAttribute('content', 'width=device-width,initial-scale=0.3333333333333333, maximum-scale=0.3333333333333333, minimum-scale=0.3333333333333333, user-scalable=no')
            }
            function resize() {
                let width = screen.width > 750 ? '75px' : screen.width / 10 + 'px'
                document.getElementsByTagName('html')[0].style.fontSize = width
            }
            window.onresize = resize
        </script>
    </body>
</html>



                     
      