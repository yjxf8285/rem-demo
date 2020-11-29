## 移动端rem适配 极简demo 
基于webpack 5打包，配合sass的rem2px函数支持

### rem适配原理
首先我们知道rem是相对于html根元素的单位,默认情况html的font-size是16px，所以 1rem = 16px；

那么得出来，pc端设计稿的话如果30px换成rem的公式就是 30/16=1.8rem（目标单位/根字体单位）；

为了适配不同的屏幕大小，在不同大小的屏幕上面，字体也要随着改变，所以我们不能只用16px，而需要动态的设定根字体的单位；

例如现在移动端的设计稿普遍使用ihpone6作为标准，也就是宽度为375，为了计算方便，首先用js写一个函数把html的字体单位设为375/10,也就是37.5，像这样：
```
  let htmlDom = document.getElementsByTagName('html')[0]
        let setFontSize = function () {
            let htmlWidth = document.documentElement.clientWidth //(viewport宽度)
            htmlDom.style.fontSize = htmlWidth / 10 + 'px' //iphone6=37.5
        }
        setFontSize()
```
具备了动态改变根字体的能力，只要给文字或者容器宽高的值设定rem就可以实现适配了；
那么根据设计稿的要求我们具体应该设置多少rem呢？这里有个公式，也非常简单，就是设计稿标注的像素值除以根字体的值就得出了rem的值
基于iPhone6的设计稿某个文字标注30px。那么它的rem值 就是30/37.5 = 0.8rem。 所以在css中的字体大小就要写.8rem。

学到这里，大家可以尝试一下，在chrome的调试看看结果，不过要注意一点就是，iphone6的屏幕是2x屏，所以在算rem的时候需要把30px看做30/2也就是15px然后再用15/37.5。现在业界已经默认都使用Iphone6最为移动端UI的标准了。

但是每次写rem之前都得自己用计算器除一次也太麻烦了，所以我们可以写一个scss的函数来帮我们这样就方便很多，例如像下面这样使用。
为了方便编译scss文件，可以使用webpack来打包编译或则其他方式。
```
@function px2rem($px) {
    $rem: 37.5px; //iphone 6
    @return ($px/$rem) + rem;
}
#main {
    font-size: px2rem(15px); 
}
```
这套源码的使用方法：
```
npm i
npm run build
```