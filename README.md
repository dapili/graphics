# 帧动画和缓动动画

## 一、背景&介绍
程序动画作为前端开发的一部分，其内容丰富（缓动动画，帧动画，骨胳动画，shader动画等）；
本次针对日常开发中帧动画和缓动动画作一简单总结介绍。

1. 帧循环动画
2. 缓动动画


## 二、目的（你会学到什么？）

1. 了解何为帧循环动画，它的适用场景
2. 添加变化，创建可控随机的一些数学方法
3. 了解gsap在pixijs中的安装使用
4. 了解一些缓动方程

## 三、内容

### I、帧动画

* 定义：每秒让物体运动多少次，关注单次变化（电影每秒24帧，游戏每秒60帧）；  
* 适用：不关注起始时间，不明确状态；随机动画，响应玩家操作  
* 缺点：性能不足的机器上可能出现比预期慢的现象

#### 1，正余弦

##### 应用：

* 1.1 利用其取值在-1到1之间，形成tween库中loop或者yoyo（溜溜球）循环往复的效果；  
* 1.2 利用其函数图形，实现物体跳跃，转盘菜单或者绘制其他多样的图形；  
* 1.3 四周飞散的粒子动画，喷泉动画（根据角度计算x,y轴分量）

##### 例子
* 正余弦 (heart,circle,spiral（螺旋）,graphics,flower)

#### 2. 随机动画 brown
* 下雨，粒子动画，各种粒子库（一定范围内的随机）

#### 3. 缓动和弹动 easing spring  
* 靠近目标点，缓动是速度v越来越慢，想像汽车停下来  
* Easing 缓动  这里的缓动算是一种最简形式，对于各种缓动库中的则有所不同 [缓动方程 line 1138](https://github.com/kittykatattack/ga/blob/master/plugins.js)
* 弹动是加速度a越来越慢，想像橡皮筋

#### 4. 弹性结点域 nodeGraden
#### 5. 在pixijs中封装帧循环 enterFrame
Pixijs中没有egret中的帧循环事件，在egret中我们可以
````javaScript
    egret.stage.addEventListener(egret.Event.enterFrame， this.onEnterFrame， this);
    function onEnterFrame(e: egret.Event) {
        foo.x += vx;
    }
````

Pixijs提供有app.ticker，我们可以封装两个方法：
````javaScript
    let enterFrameFuns = [];
    function addEnterFrame(fun) {
      if (enterFrameFuns.indexOf(fun) == -1) {
        enterFrameFuns.push(fun);
      }
    }

    function removeEnterFrame(fun) {
      let index = enterFrameFuns.indexOf(fun);
      if (index != -1) {
        enterFrameFuns.splice(index， 1);
      }
    }
````

注意添加进去的函数可能出现的顺序依赖问题；

#### 6. setInterel和setTimeout注意
“这两个方法会以指定的时间间隔触发指定的方法，但是这两个方法的缺点是在触发指定方法时不会考虑当前正在发生的事情。即使动画没有显示或者隐藏在某个标签下，这两个函数依然会触发指定的方法，这样就会耗费某些资源。除此之外，这两个函数还存在一个缺点，那就是它们一旦被调用就会刷新屏幕，不管这个时刻对浏览器来说是不是恰当的时机，这也就意味着较高的CPU使用率”（window.requestAnimationFrame）。

### II、缓动动画

* 定义：用多少时间让物体运动多远，关注一段时间状态的改变（3秒1000px)
* 适用：明确起始时间，状态的动画
* 缺点：引入缓动库增加额外的下载大小，不便共用（from or to）
#### gsap3和pixijs
1. gsap3介绍 [来源 演变](https://segmentfault.com/a/1190000005366176)
2. 安装使用(有无插件) gsapInstall
3. 进度条实现 progressBar
4. 任意路径实现 anyPath [flash 引导动画](https://www.jb51.net/flash/base/179152.html)


## 四、总结&结论

* 对于帧动画的一些应用，不难发现它们的一个共性就是量大，不论是每帧驱动的粒子数和绘制的线条数，这里需要考虑性能问题；尽量将相关的帧动画，放入同一帧循环中，动画完成后，一定要停止动画并释放相关资源；
* 对于添加变化除正余弦，可有平方，立方（改变粒子分布），乘除（scaleX）等
* 缓动效果不明显，加大时间试试(bounsOut)
* 帧动画和缓动动画都有各自合适的应用场景


## 五、其他

1. 《flash actionScript 3.0 动画教程》 美Keith Peters著
2. [gsap3各缓动函数在线试验场](https://greensock.com/docs/v3/Eases)
