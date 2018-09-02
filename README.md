# 约束布局详解
### 1、什么是ConstraintLayout？

#### ConstraintLayout：
约束布局，可以理解为Relativelayout的加强版，在相对布局的基础上加上了LinearLayout和百分比布局的相关属性

**优势：**

大大减少了布局的层级，减少布局渲染时间
### 2、使用前准备

### 3、基本属性，相对定位

 + layout_constraintLeft_toLeftOf
 + layout_constraintLeft_toRightOf
 + ... ...

属性的格式都为layout_contraintXXX_toYYYof的格式

**contraintXXX：** 表示当前控件的哪一条边（具体值为：left\top\right\bottom\baseline，baseline为基准线对齐）

**toYYYof ：** 表示目标的控件的YYY侧（具体值为：left\top\right\bottom\baseline，baseline为基准线对齐）

如：app:layout_constraintRight_toLeftOf="@id/btn02"

具体表示当前控件的右边 位于  id为btn02的目标控件  的左侧，即当前控件的下方与目标控件的右侧发生约束关系

当XXX与YYY相同的时候，表示当前控件和目标控件的这一条边对齐

如：app:layout_constraintLeft_toLeftOf="@id/btn01"

表示当前控件与 id为btn01的目标控件 左侧对齐


```
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <!--跟父控件的左侧对齐，位于btn02的左边-->
    <Button
        android:id="@+id/btn01"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toLeftOf="@id/btn02"
        android:text="1"/>

    <!--位于btn01的右边-->
    <Button
        android:id="@+id/btn02"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:layout_constraintLeft_toRightOf="@id/btn01"
        android:text="2"/>

    <!--位于btn01的下方-->
    <Button
        android:id="@+id/btn03"
        android:layout_width="100dp"
        android:layout_height="70dp"
        app:layout_constraintTop_toBottomOf="@id/btn01"
        android:text="3"
        android:textSize="30sp"/>

    <!--跟父控件的右侧对齐-->
    <Button
        android:id="@+id/btn04"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:layout_constraintRight_toRightOf="parent"
        android:text="4"/>

    <!--位于btn03的右边-->
    <!--该控件上方与下方分别于btn03控件的上下方对齐，但是因为高度不一样，所以自动上下居中-->
    <Button
        android:id="@+id/btn05"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:layout_constraintTop_toTopOf="@id/btn03"
        app:layout_constraintBottom_toBottomOf="@id/btn03"
        app:layout_constraintLeft_toRightOf="@id/btn03"
        android:text="5"/>

    <!--baseline基准线对齐-->
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="6"
        app:layout_constraintLeft_toRightOf="@+id/btn05"
        app:layout_constraintTop_toBottomOf="@id/btn01"
        app:layout_constraintBaseline_toBaselineOf="@id/btn03"/>

</android.support.constraint.ConstraintLayout>
```
具体UI如下：

![image](http://ozta3wbsi.bkt.clouddn.com/android/blogImage/constraintlayout/%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8.png)

### 4、居中定位和偏移bias

在说到控件自身居中，首先要说一下上边讲述的属性的属性值还有一个parent，如：

app:layout_constraintRight_toRightOf="parent"

表示该控右侧件与父控件的右侧对齐，即右边与父控件右边对齐

当一个控件同时出现：上下或者左右对齐，然后上下的长度或者左右的宽度又跟对齐控件不等的时候

如：


```
<android.support.constraint.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <!--位于父控件的中心位置-->
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="center"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"/>

</android.support.constraint.ConstraintLayout>
```
上面这种情况，button的左侧与父控件的左侧对齐，右侧与父控件的右侧对齐，但是该button的宽度是wrap_content，如果达不到父控件的宽度，则造成两个约束条件对立，无法同时满足两个约束条件

实际上，采用这样的布局后，结果是该控件会左右居中，效果如图：

![对立约束，居中](http://ozta3wbsi.bkt.clouddn.com/android/blogImage/constraintlayout/%E7%BA%A6%E6%9D%9F%E5%B1%85%E4%B8%AD.png)


于是，在这种居中的情况下，就出现了偏移量的属性（bias），能够使该控件的约束偏向某一边

**bias：**
layout_constraintHorizontal_bias：横向偏移

layout_constraintVertical_bias：横向偏移

偏移的值范围都是0-1,0是左边/上边，1是右边/下边,即数值表示偏移的百分比，如下：


```
<android.support.constraint.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintHorizontal_bias="0.2"/>

</android.support.constraint.ConstraintLayout>
```
0.2表示在水平方向上往左偏移20%，具体图示：

![bias（偏移）](http://ozta3wbsi.bkt.clouddn.com/android/blogImage/constraintlayout/%E5%81%8F%E7%A7%BB%E9%87%8F.png)

### 5、强制约束：layout_constrainedWidth

使用ConstraintLayout布局的时候，当控件自身的宽度大于约束的宽度的时候，同样会使得约束条件失效，如下：


```
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="wrap_content">

    <Button
        android:id="@+id/btn01"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="XXXXXXXXXXXXXXXX"/>

    <Button
        android:id="@+id/btn02"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="YYYYYYYYYYYYYYYYYYYYYYYYYYYYY"
        app:layout_constraintLeft_toRightOf="@id/btn01"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/btn01" />

</android.support.constraint.ConstraintLayout>
```

效果图为：

![约束失效](http://ozta3wbsi.bkt.clouddn.com/android/blogImage/constraintlayout/%E7%BA%A6%E6%9D%9F%E5%A4%B1%E6%95%88.png)

如上，btn2的宽度应该在btn1的右边，同时右侧与父控件右侧对齐，不能同时满足，所以水平居中

因此，btn2的约束条件的宽度最大不能超过btn1右侧的总宽度

但是，显然，随着Button的文字加长，button的宽度也会一直变长，当btn2的宽度超过约束的宽度后，则约束条件失效

为了避免这样的情况，新增了一个属性为layout_constrainedWidth，属性值为true和false，默认为false，如此可以保证约束条件不会失效

加了layout_constrainedWidth后的效果图：

![image](http://ozta3wbsi.bkt.clouddn.com/android/blogImage/constraintlayout/%E5%BC%BA%E5%88%B6%E7%BA%A6%E6%9D%9F.png)


### 6、圆形布局circle

这种布局方式在其他的布局中是很难实现的，但是相对应的，用的也不是很多，具体看效果图：

![image](http://ozta3wbsi.bkt.clouddn.com/android/blogImage/constraintlayout/%E5%9C%86%E5%BD%A2%E5%B8%83%E5%B1%80.png)

代码如下：

```
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <Button
        android:id="@+id/btn01"
        android:layout_width="400dp"
        android:layout_height="300dp"
        android:text="center"
        android:background="@color/green"/>

    <Button
        android:id="@+id/btn_circle"
        android:layout_width="300dp"
        android:layout_height="300dp"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        android:text="center"
        android:background="@drawable/shape_const_circle"/>

    <Button
        android:id="@+id/btn02"
        android:layout_width="100dp"
        android:layout_height="60dp"
        android:background="@color/red"
        android:text="test"
        app:layout_constraintCircle="@id/btn01"
        app:layout_constraintCircleRadius="80dp"
        app:layout_constraintCircleAngle="45"/>

    <Button
        android:id="@+id/btn_test"
        android:layout_width="60dp"
        android:layout_height="60dp"
        android:background="@drawable/shape_const_circle"
        android:text="test"
        app:layout_constraintCircle="@id/btn01"
        app:layout_constraintCircleRadius="80dp"
        app:layout_constraintCircleAngle="45"
        android:visibility="gone"/>


    <!--圆形布局-->
    <!-- app:layout_constraintCircle:指定参照的控件id，（参照控件用于约束 约束控件的位置）,此处指定btn02控件约束于btn01控件的中心-->
    <!-- app:layout_constraintCircleRadius:指定约束的控件中心与参照控件中心的距离（半径）-->
    <!-- app:layout_constraintCircleAngle:指定约束的控件中心与参照控件中心的角度（偏移量）-->


</android.support.constraint.ConstraintLayout>
```
圆形布局中有三个属性，分别为：
+ app:layout_constraintCircle:"@id/xxx"，该控件的约束控件的id，即根据xxx控件产生的约束条件
+ app:layout_constraintCircleRadius:"xxx"，指定该控件中心与约束控件中心距离，即半径
+ app:layout_constraintCircleAngle:"xxx"，指定该控件中心与约束控件中心的偏移角度（竖直的上方为0°，水平右侧为90°）

### 7、百分比约束（针对父控件的百分比）

约束布局中还提供了控件占据父控件宽高的百分比的属性，具体如下：

```
<android.support.constraint.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <Button
        android:layout_width="0dp"
        android:layout_height="0dp"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintHeight_percent="0.2"
        app:layout_constraintWidth_percent="0.5"/>

    <!--在约束布局中，0dp代表match_parent-->
    <!--layout_constraintHeight_percent表示百分比，相对于父控件的宽、高-->

</android.support.constraint.ConstraintLayout>
```

app:layout_constraintHeight_percent="0.5"，表示该控件占据父控件高度的50%，如图：

![image](http://ozta3wbsi.bkt.clouddn.com/android/blogImage/constraintlayout/%E7%88%B6%E5%AE%B9%E5%99%A8%E7%99%BE%E5%88%86%E6%AF%94%E5%A1%AB%E5%85%85.png)

当然，说到这里，可能就需要说一下在约束布局中，控件的宽高属性值有三种方式：

+ 固定宽高值
+ wrap_content
+ 0dp（也就是match_contraint）

注意，约束布局中宽高属性没有match_parent属性值了，被0dp代替，

并且，0dp用在width上，则水平方向必须有约束，不然失效，同理，用在height上，竖直方向必须有约束,因为0dp（match_constraint是用在约束上面的，没有约束，0dp是不成立的）

具体，看代码：

```
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <Button
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:text="btnTest"/>

</android.support.constraint.ConstraintLayout>
```
此时看到的效果是。button还是默认的大小，宽度并没有充满父布局，因为水平方向没有约束，match_constraint失效，我们只需要加上：

```
app:layout_constraintLeft_toLeftOf="parent"
app:layout_constraintRight_toRightOf="parent"
```
效果就出来了。由此可以得知，约束一定要和0dp（match_constraint）的方向一致，不然会失效


### 8、Chains
> 在同一个轴上，一组控件通过一个双向的约束关系关联起来，并且链条的属性由链条头（横向的最左端或者纵向的最顶端）的控件控制

所谓双向约束关系，即两个控件相互成为约束条件，代码如下：

```
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <Button
        android:id="@+id/btn01"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="1"
        app:layout_constraintRight_toLeftOf="@+id/btn02"/>

    <Button
        android:id="@+id/btn02"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="2"
        app:layout_constraintLeft_toRightOf="@id/btn01"/>

</android.support.constraint.ConstraintLayout>
```
由代码可以看出，双向约束关系，就是通过btn01的app:layout_constraintRight_toLeftOf="@+id/btn02"和btn02的app:layout_constraintLeft_toRightOf="@id/btn01"达到的一个双向关系。

chains有三种样式：
+ packed 链式控件横向或者纵向自然的平铺（默认的样式）
+ spread 链式控件默认展开居中，平分可用空间
+ spread_inside 除开链式控件的两个端点控件，其余的控件平分剩余空间

不多说，引用网上的一图片如下：

![chains](https://upload-images.jianshu.io/upload_images/2258857-5803cbea741358a4?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

当然，chains链条是区分横向和纵向的，上边的图片只是横向的样式，纵向的同理

需要注意的是，其中有两点需要注意：
 1. 当链条中的控件如果有设置margin的话，是需要被考虑在内的，即在分配剩余空间的时候，设置了margin的控件是需要当做该控件的一部分
 2. 当链条中的控件如果有一个控件使用了0dp（match_constraint），则该控件铺满剩余的空间；
 3. 当链条中的控件如果有多个控件使用了0dp，同时在没有控件使用layout_constraintXXX_weight（XXX表示横向或者纵向）属性，则使用了0dp的控件平均分配占据剩余空间
 4. 当链条中的控件如果有多个控件使用了0dp，并且使用了layout_constraintXXX_weight属性，则使用了0dp的控件按照weight权重分配占据剩余空间

具体看一下效果图：

![image](http://ozta3wbsi.bkt.clouddn.com/android/blogImage/constraintlayout/%E9%93%BE%E5%BC%8F%E5%88%86%E5%B8%83.png)



### 9、Ratio比例
> 在约束布局中，当前控件宽或者高确定的话，可以根据比例确定另一边的大小，通过layout_constraintDimensionRatio属性来设定宽高比例

该属性有三种设定方式：
+ layout_constraintDimensionRatio="1:2" 默认值
+ layout_constraintDimensionRatio="W,1:2"
+ layout_constraintDimensionRatio="H,1:2"

当约束宽或者高其中一个为0dp，另一边设为固定dp值的时候，如下：

```
 <Button
        android:id="@+id/btn02"
        android:layout_width="200dp"
        android:layout_height="0dp"
        android:text="1"
        app:layout_constraintTop_toBottomOf="@+id/btn01"
        app:layout_constraintDimensionRatio="1:2"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"/>
```
当前控件的宽度确定为200dp，高度为0dp（即match_contraint约束），宽高比例为宽：高=1:2，当ration中没有W和H的时候，默认为宽：高。

当同时约束宽和高都为0dp的时候，如下：

```
<Button
        android:layout_width="0dp"
        android:layout_height="0dp"
        android:layout_marginLeft="20dp"
        android:layout_marginRight="20dp"
        android:text="3"
        app:layout_constraintDimensionRatio="w,1:2"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toBottomOf="@id/btn03" />
```
我们先看一下效果图：

![ratio比例之w](http://ozta3wbsi.bkt.clouddn.com/ratio%E6%AF%94%E4%BE%8Bw.png)

通过效果图可以看到，此时宽高的比例是宽：高=2:1
现在我们将ratio的属性值中的w改成h，我们再来看看效果图，如下：

![ratio比例之h](http://ozta3wbsi.bkt.clouddn.com/ratio%E6%AF%94%E4%BE%8Bh.png)

通过效果图可以看到，此时宽高的比例是宽：高 = 1:2

其实，从该控件的约束条件来看，宽度实际上已经确定了，为填满横屏，因此，比例都是宽度来伸缩的

由此可以得出关于ratio属性中w和h结论：(ratio表示比例)

当Ratio属性值为w,ratio时：此时的比例ratio = 高：宽

当Ratio属性值中有h,ratio时：此时的比例ratio = 宽：高

总结：
+ Ratio属性只有比例值ratio，则默认表示宽：高
+ Ratio属性比例值为w,ratio，ratio表示高：宽
+ Ratio属性比例值为h,ratio,ratio表示宽：高

### 10、goneMargin
> 在我们app的一些需求上，通查会有一些控件需要显示和隐藏，而往往有我们将该控件设置为隐藏状态后，与它相关联的控件的位置也会发生改变，从而影响到整体的布局。而在约束布局中，goneMargin就是为解决这一问题而存在的。

话不多说，直接上代码：

```
    <Button
        android:id="@+id/btn01"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:layout_constraintRight_toLeftOf="@id/btn02"
        app:layout_constraintLeft_toLeftOf="parent"
        android:text="1"
        android:visibility="gone"/>

    <Button
        android:id="@+id/btn02"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:layout_constraintLeft_toRightOf="@id/btn01"
        app:layout_constraintRight_toLeftOf="@id/btn03"
        android:text="2"
        app:layout_goneMarginLeft="20dp"/>
```
当btn01控件的状态为GONE的时候，btn02的goneMargin属性生效

如果btn01隐藏之后，想要维持btn02的位置不变，只需要将layout_goneMarginLeft设置为btn01的宽度加上左右margin即可

### 11、辅助布局

##### Guideline
+ 对于从事android开发的人来说，适配通常是一个令人头疼的问题，以往的布局适配，我们可能更多的是使用weight权重来解决部分适配问题，但是与此同时，使用了权重，会影响渲染，而且嵌套的层级也会越来越多，不仅影响性能，还不易维护。
+ 因此，在约束布局中，加入了这种更实用的Guideline进行布局的辅助
+ guideline使用（两个方向）
    + horizontal 横向，高度为0
    + vertical 纵向，宽度为0
+ 属性值：
    + layout_constraintGuide_begin 距离父布局起始位置（左或者上）的距离
    + layout_constraintGuide_end 距离父布局结束位置（右或者下）的距离
    + layout_constraintGuide_percent 百分比位置（位于父布局横向或者纵向）

假如现在有这样一个布局，如图：

![image](http://ozta3wbsi.bkt.clouddn.com/android/blogImage/constraintlayout/exp_resouce.png)

这种情况下我们传统的做法应该是使用Linearlayout和RelativeLayout进行各种嵌套，一套布局写下来，既费时间，还费性能，而如果使用Guideline的话，如下：

```
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="170dp"
    tools:layout_editor_absoluteY="25dp">

    <android.support.constraint.Guideline
        android:id="@+id/guideline3"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        app:layout_constraintGuide_end="192dp" />

    <android.support.constraint.Guideline
        android:id="@+id/guideline"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        app:layout_constraintGuide_percent="0.5" />

    <ImageView
        android:id="@+id/imageView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="8dp"
        android:layout_marginTop="8dp"
        app:layout_constraintBottom_toTopOf="@+id/guideline"
        app:layout_constraintTop_toTopOf="parent"
        app:srcCompat="@mipmap/ic_launcher"
        tools:layout_editor_absoluteX="199dp" />

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="2dp"
        android:text="卡卡西"
        app:layout_constraintBottom_toBottomOf="@+id/textView3"
        app:layout_constraintTop_toTopOf="@+id/textView3"
        app:layout_constraintVertical_bias="1.0"
        tools:layout_editor_absoluteX="340dp" />

    <TextView
        android:id="@+id/textView2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="TextView"
        app:layout_constraintStart_toStartOf="@+id/textView3"
        tools:layout_editor_absoluteY="57dp" />

    <TextView
        android:id="@+id/textView3"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="168.00"
        tools:layout_editor_absoluteX="282dp"
        tools:layout_editor_absoluteY="16dp" />

    <ImageView
        android:id="@+id/imageView2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="8dp"
        android:layout_marginTop="8dp"
        app:layout_constraintBottom_toTopOf="@+id/guideline"
        app:layout_constraintTop_toTopOf="parent"
        app:srcCompat="@mipmap/ic_launcher"
        tools:layout_editor_absoluteX="117dp" />

    <TextView
        android:id="@+id/textView4"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="126.0"
        tools:layout_editor_absoluteX="16dp"
        tools:layout_editor_absoluteY="16dp" />

    <TextView
        android:id="@+id/textView5"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="水润肤"
        tools:layout_editor_absoluteX="59dp"
        tools:layout_editor_absoluteY="16dp" />

    <TextView
        android:id="@+id/textView6"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="TextView"
        tools:layout_editor_absoluteX="16dp"
        tools:layout_editor_absoluteY="50dp" />

    <TextView
        android:id="@+id/textView7"
        android:layout_width="80dp"
        android:layout_height="24dp"
        android:layout_marginBottom="8dp"
        android:layout_marginEnd="8dp"
        android:layout_marginLeft="8dp"
        android:layout_marginRight="8dp"
        android:layout_marginStart="8dp"
        android:layout_marginTop="8dp"
        android:gravity="center"
        android:text="点击购买"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toStartOf="@+id/guideline3"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/guideline" />

    <ImageView
        android:id="@+id/imageView4"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="8dp"
        android:layout_marginTop="8dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintStart_toStartOf="@+id/imageView"
        app:layout_constraintTop_toTopOf="@+id/guideline"
        app:layout_constraintVertical_bias="0.0"
        app:srcCompat="@mipmap/ic_launcher" />

    <TextView
        android:id="@+id/textView8"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="22.00"
        tools:layout_editor_absoluteX="280dp"
        tools:layout_editor_absoluteY="103dp" />

    <TextView
        android:id="@+id/textView9"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="反正测试"
        tools:layout_editor_absoluteX="322dp"
        tools:layout_editor_absoluteY="103dp" />

    <TextView
        android:id="@+id/textView10"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="TextView"
        app:layout_constraintStart_toStartOf="@+id/textView8"
        tools:layout_editor_absoluteY="135dp" />

</android.support.constraint.ConstraintLayout>
```
效果图如下：

![image](http://ozta3wbsi.bkt.clouddn.com/android/blogImage/constraintlayout/gudeline%E8%BE%85%E5%8A%A9%E5%B8%83%E5%B1%80.png)

由此可见，guideline用于约束布局中的定位辅助

###### Barrier

+ 用一组view来限定自身位置的一种辅助线，限制源是一组view的id值
+ 属性
    + constraint_referenced_ids 限制源，值由一组id值组成，id值之间使用都好隔开
    + barrierDirection 指定限制的方向（有left\top\right\bottom\start\end六组值），根据限制源来限定的，如该属性值为right的时候，表示该Barrier位于ids这<font color=red>一组控件</font>的最右侧

接下来看一下实例效果，我们通常做的app里面一半都会有这样的页面，左边标题，右边输入框，如登录页面，当这样的输入条目过多的时候，因为前面标题的文字个数可能不一样，想要保证输入框对齐，按原本的写法，需要去调整每一个标题的长度才能实现，如果通过Barrier来实现的话，相对来说会方便很多，如下：

```
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <TextView
        android:id="@+id/tv_name"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginLeft="15dp"
        app:layout_constraintTop_toTopOf="@id/et_name"
        app:layout_constraintBottom_toBottomOf="@id/et_name"
        app:layout_constraintLeft_toLeftOf="parent"
        android:text="登录名："/>
    <TextView
        android:id="@+id/tv_password"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:layout_constraintLeft_toLeftOf="parent"
        android:layout_marginLeft="15dp"
        app:layout_constraintTop_toBottomOf="@id/tv_name"
        app:layout_constraintTop_toTopOf="@id/et_password"
        app:layout_constraintBottom_toBottomOf="@id/et_password"
        android:text="密码："/>
    <EditText
        android:id="@+id/et_name"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginRight="15dp"
        app:layout_constraintLeft_toRightOf="@id/barrier"
        app:layout_constraintRight_toRightOf="parent"
        android:hint="请输入您的登录名"/>
    <EditText
        android:id="@+id/et_password"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginRight="15dp"
        app:layout_constraintLeft_toRightOf="@id/barrier"
        app:layout_constraintTop_toBottomOf="@id/et_name"
        app:layout_constraintRight_toRightOf="parent"
        android:hint="请输入您的密码"/>

    <android.support.constraint.Barrier
        android:id="@+id/barrier"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:constraint_referenced_ids="tv_name,tv_password"
        app:barrierDirection="right"/>

    <android.support.constraint.Guideline
        android:layout_width="wrap_content"
        android:layout_height="wrap_content" />

</android.support.constraint.ConstraintLayout>
```
效果图如下：

![image](http://ozta3wbsi.bkt.clouddn.com/android/blogImage/constraintlayout/barrier%E8%BE%85%E5%8A%A9%E5%B8%83%E5%B1%80.png)

如此，我们所有的输入框只需要位于barrier的后边就可以了，而barrier位于ids组的控件的最右边

###### Group
用于控制一组控件的可见性

+ constraint_referenced_ids 控件的id组合
+ visibility="gone" 用于控制控件组id对应的所有控件的可见性

具体如下：
```
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <android.support.constraint.Group
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:visibility="gone"
        app:constraint_referenced_ids="btn01,btn02"/>

    <Button
        android:id="@+id/btn01"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toLeftOf="@id/btn02"
        android:text="1"/>
    <Button
        android:id="@+id/btn02"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:layout_constraintLeft_toRightOf="@id/btn01"
        android:text="2"/>

    <!--android.support.constraint.Group  用于多个控件的显示隐藏-->
    <!--visibility 显示/隐藏-->
    <!--constraint_referenced_ids  要控制的控件组的id集合-->

</android.support.constraint.ConstraintLayout>
```

源码查看[点击这里](https://github.com/VinnyChen/ConstraintLayoutDemo)

