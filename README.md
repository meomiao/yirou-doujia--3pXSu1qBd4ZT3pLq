
# 一.动画原理和应用


three的动画大概就是通过不同时间的关键帧来实现


加载一个手机模型


[![](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412122217598.png)](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412122217598.png)


在这个对象里面，**注意后期都是直接通过可视化软件Blender编辑好关键帧就能实现动画**，这也是个已经编辑好的动画模型，在这个对象里面有一个animations就是动画集，也就是这个物体可以有很多个动画


其中animationclip表示剪辑动画，duration表示动画时长，tracks轨道表示各种关键帧，比如第一个关键帧里面，name什么group1 2 3等等都是一个部位，最后position表示这个关键帧是移动了位置，times表示时间，values是顶点，三维向量，一个时间对应values三个向量也就是一个顶点，表示这个时间段0\.03999999910593033秒，第一个点移动到了\-0\.009195199236273766, \-0\.021173708140850067, \-0\.05099957436323166这个位置，所以times x3 等于values


[![](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412122224089.png)](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412122224089.png)


当然你要看物体也可以在children属性里面看到


[![](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412122226066.png)](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412122226066.png)


编辑好后拿到模型此时是静态的，还需要将动画播放起来


[![](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412122228556.png)](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412122228556.png)


**此时还要注意，我们要不断更新动画，那就在animate函数里面，并且创建一个时间，作为参数，要让动画知道时间是什么**


[![](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412122231029.png)](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412122231029.png)


[![](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412122232330.gif)](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412122232330.gif)


## 1\.1 代码实现keyframme关键帧动画


简单创建一个立方体


[![](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412122250252.png)](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412122250252.png)


[![](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412122251687.gif)](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412122251687.gif)


## 1\.2 四元数与欧拉角转换设置关键帧


主要就是旋转动画，刚才是位移动画，旋转动画就要用到QuaternionKeyframeTrack


四元数就是利用一系列公式算出角度可以避免旋转过程中轴心偏向的问题


当然这个three已经帮我们处理好了，我们不用去算直接用她的一个数学库来解决


[![](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412122257940.png)](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412122257940.png)


[![](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412122308972.png)](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412122308972.png)


[![](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412122308044.gif)](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412122308044.gif)


**除此之外，四元数还支持欧拉角的方式来设置角度，三个参数分别表示x轴角度y轴z轴**


[![](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412122310515.png)](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412122310515.png)


## 1\.3 布尔关键帧动画


就是控制显示隐藏，类似于星星闪烁效果


[![](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412162234624.png)](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412162234624.png)


[![](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412162234325.gif)](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412162234325.gif)


**如果要对模型使用，注意模型的名字可以通过children属性查找里面的name**


[![](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412162242803.png)](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412162242803.png)


[![](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412162242176.gif)](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412162242176.gif)


## 1\.4 颜色关键帧


可以根据时间设置不同的颜色


**注意颜色支持rgb但是是0\-1的写法**


而且是对材质设置


[![](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412162249038.png)](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412162249038.png)


[![](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412162250728.gif)](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412162250728.gif)


## 4\.5 数值关键帧


就是想对这个3D物体某些属性进行数值上的单独修改可以用这个


**比如透明度**


**注意要找到模型的材质，可以一直往这个gltf.scene的children下面找，找到最后一级会有mesh此时就带有材质**


[![](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412162302060.png)](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412162302060.png)


[![](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412162303197.gif)](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412162303197.gif)


# 二.混合器


混合器就是可以对动画进行一些设置，比如时间多少，快慢，暂停启动等


[![](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412162311793.png)](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412162311793.png):[slower加速器官网](https://chundaotian.com)


[![](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412162312648.gif)](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412162312648.gif)


# 三.人物多动作丝滑切换


加载模型进来，可以看到这是一个已经做好动画的模型


[![](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412182245352.png)](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412182245352.png)


[![](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412182246465.png)](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412182246465.png)


随便取几个代表性的动画出来


[![](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412182256384.png)](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412182256384.png)


**注意：这些动画不能一股脑一次性播放不然会变得很鬼畜**


[![](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412182257331.png)](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412182257331.png)


首先来一个站着不动呼吸的动画


其他效果类似


[![](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412182311752.png)](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412182311752.png)


[![](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412182312615.gif)](https://heymar.oss-cn-chengdu.aliyuncs.com/undefined202412182312615.gif)


  * [一.动画原理和应用](#%E4%B8%80%E5%8A%A8%E7%94%BB%E5%8E%9F%E7%90%86%E5%92%8C%E5%BA%94%E7%94%A8)
* [1\.1 代码实现keyframme关键帧动画](#tid-YQFDRe)
* [1\.2 四元数与欧拉角转换设置关键帧](#tid-d6QiJJ)
* [1\.3 布尔关键帧动画](#tid-CA8Xf8)
* [1\.4 颜色关键帧](#tid-xRN3kp)
* [4\.5 数值关键帧](#tid-rTDf58)
* [二.混合器](#%E4%BA%8C%E6%B7%B7%E5%90%88%E5%99%A8)
* [三.人物多动作丝滑切换](#%E4%B8%89%E4%BA%BA%E7%89%A9%E5%A4%9A%E5%8A%A8%E4%BD%9C%E4%B8%9D%E6%BB%91%E5%88%87%E6%8D%A2)

   \_\_EOF\_\_

   ![](https://github.com/heymar)Heymar  - **本文链接：** [https://github.com/heymar/p/18631662](https://github.com)
 - **关于博主：** 评论和私信会在第一时间回复。或者[直接私信](https://github.com)我。
 - **版权声明：** 本博客所有文章除特别声明外，均采用 [BY\-NC\-SA](https://github.com "BY-NC-SA") 许可协议。转载请注明出处！
 - **声援博主：** 如果您觉得文章对您有帮助，可以点击文章右下角**【[推荐](javascript:void(0);)】**一下。
     
