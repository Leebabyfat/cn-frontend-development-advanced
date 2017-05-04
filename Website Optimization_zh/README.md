## 网站性能优化项目

### 如何开始此应用？

以下是几个帮助你顺利开始本项目的提示：

1. 将这个代码库导出
2. 你可以运行一个本地服务器，以便在你的手机上检查这个站点

```bash
  $> cd /你的工程目录
  $> python -m SimpleHTTPServer 8080
```

1. 打开浏览器，访问 localhost:8080
2. 下载 [ngrok](https://ngrok.com/) 并将其安装在你的工程根目录下，让你的本地服务器能够被远程访问。

``` bash
  $> cd /你的工程目录
  $> ./ngrok http 8080
```

1. 复制ngrok提供给你的公共URL，或者将此项目部署到tomcat服务器上，通过localhost:8080/你的地址/ 来打开index.html
2. 打开后，可以点击Build Your Own 2048!、Website Performance Optimization、Mobile Web Development、Cam's Pizzeria这四个网页的详细介绍。并且，在Cam's Pizzeria这个中，可以使用拖动条，来改变pizza的大小。
----


### 优化概述

### index.html的优化
       在index.html的优化中，我首先使用Chrome安装了PageSpeed插件，并使用该插件对index.html进行测试，根据测试结果对pizza.jpg和pizzria.jpg进行了优化压缩，并且，对于字体进行了优化，使用了异步加载。

### pizza.html的优化---修改main.js
  -  首先，使用requestAnimationFrame(updatePositions)，可以优化并行的动画动作，更合理的重新排列动作序列，并把能够合并的动作放在一个渲染周期内完成，从而呈现出更流畅的动画效果；
  - 防止同步布局：
     1 . 此处会由于读写都在一个for循环中，因此会导致频繁读写，也就是不停地进行Recalculate Styels 和 layout，因此需要将此处进行批量读取和批量写入，提高性能。
        ```
         for (var i = 0; i < items.length; i++) {
            phase[i] = Math.sin((document.body.scrollTop / 1250) + (i % 5));
            items[i].style.left = items[i].basicLeft + 100 * phase[i] + 'px';
        }
         ```
      
     2 . 原来的changePizzaSizes函数中出现了三次document.querySelectorAll(".randomPizzaContainer"),为了不重复这么多的代码，因此将这些DOM节点集合保存到数组中，方便在for循环中调用。其次更影响性能的一点是determineDx，determineDx没有存在的必要，而且这个determineDx函数在变换pizza的尺寸时，会因为在循环中导致同步布局，产生大量的计算布局计算布局情况，因此，将不需要的函数去掉，直接转百分比。
        
        
        function changePizzaSizes(size) {
          switch(size) {
            case "1":
              newWidth = 25;
              break;
            case "2":
              newWidth = 33.3;
              break;
            case "3":
              newWidth = 50;
              break;
            default:
              console.log("bug in sizeSwitcher");
          }
              var randomPizzas =  document.getElementsByClassName("randomPizzaContainer");
              for (var i = 0; i < randomPizzas.length; i++) {
                randomPizzas[i].style.width = newWidth + "%";
              }
        } 
    
 - 使用运行速度更快的选择器，使用getElementById替代querySelector，getElementByClassName代替querySelectorAll

### 请联系我
      对于此项目，您有什么意见和建议欢迎发邮件与我联络，我的邮箱是653982905@qq.com。
      另外，我在尝试使用
   ```
       items[i].style.transform = 'translateX('  + 100  * phase[i] + 'px)' ;
   ```
        去代替main.js中的494行，防止reflow的时候，出现一些小问题，如果你对此有什么解决办法，请您不吝赐教。

