# react-native-pg-style

以最简单的方式编写样式代码,抛弃react-native标准的样式创建方式.

看大家写的源码中都是按照react-native标准的样式创建方式来写样式代码的,样式代码就占了大概四分之一,甚至三分之一的代码,然而我却喜欢把样式写在一行当中.而不用`const styles=StyleSheet.create({样式属性...})`来写,我觉得这样在改动样式时便不用在跑到`StyleSheet.create`中修改,而且代码量会少很多,于是就有了这个插件.

`下面说明中的插件就是当前这个react-native-pg-style插件`

github地址: https://github.com/geek-prince/react-native-gp-style

npm地址: https://www.npmjs.com/package/react-native-gp-style


## 先简单感受一下
`以侵入式等级1为例,什么是侵入式等下下面再介绍`

![(一个简单的按钮)](http://github.jikeclub.com/gpStyle/image1.png)

标准方法创建该样式为:

```jsx
const styles=StyleSheet.create({
    container:{
        padding:15,
        width:Dimensions.get('window').width,
        backgroundColor:'#999',
        marginTop:100,
    },
    button:{
        width:'100%',
        height:50,
        backgroundColor:'#f90',
        justifyContent:'center',
        alignItems:'center',
        borderColor:'#0f9',
        borderRadius:25,
        borderWidth:2,
    },
    btnText:{
        backgroundColor:'#0000',
        color:'#fff',
        fontSize:20,
    },
});

<View style={styles.container}>
  <TouchableOpacity style={styles.button}>
      <Text style={styles.btnText}>有本事点我呀</Text>
  </TouchableOpacity>
</View>
```

很长的一段代码对吧,那再来看看用了这个插件之后的代码为:

```jsx
<View style={[sf.spad(15),sf.sw(s.w),s.bg999,sf.smarT(100)]}>
  <TouchableOpacity style={[s.w100,sf.sh(50),sf.sbgc('#f90'),s.center,sf.sb('#0f9',25,2)]}>
      <Text style={[sf.stext(20,'#fff',)]}>有本事点我呀</Text>
  </TouchableOpacity>
</View>
```

直接发样式写在一行内,完全抛弃`const styles=StyleSheet.create`这样的方法,要修改时直接在组件这里就改了,不用跑到`const styles=StyleSheet.create`中去改,即使代码写在一行内也简短不显得臃肿.


## 安装
`npm install react-native-gp-style --save`


## 如何使用

首先导入插件

### 导入插件的三种方式(侵入方式的三个等级,根据情况选择其中一种方式导入)
先导入插件

`import Styles from 'react-native-gp-style';`

然后根据情况(看下面的三种方式的优缺点)选择下面的一种方式使用插件.

- 非侵入式(侵入等级0):

`let {s,sf}=new Styles(0);`

或自定义名称

`let {s:styles,sf:styleFunctions}=new Styles(0);`

- 部分侵入式(侵入等级1,推荐):

`new Styles(1);`

或自定义名称

`new Styles(1,'styles','styleFunctions');`

- 完全侵入式(侵入等级2):  

`new Styles(2);`

或自定义名称

`new Styles(2,'styles');`

### 怎样使用
像上面一样导入之后有两个变量s,sf(侵入式导入的话存放在global全局变量中).

- s是一个对象,这个对象中以简单的键名key对应到一个固定的样式.
-- 比如`s.center`对应的是子组件水平,垂直居中的样式`{justifyContent:'center',alignItems:'center'}`.
-- 比如`s.w`对应的是当前设备的宽度(侵入式等级2时,直接w)`{width:Dimensions.get('window').width}`.

- sf也是一个对象,这个对象中以简单的键名key对应到一个获取样式的方法.可以通过调用这些方法,传入方法参数,获得对应的样式.
-- 比如`sf.swh(100,200)`就获取到宽100,高200的样式(可以只设置一个值,此时宽高都等于这个值)`{width:10,height:20}`.(侵入式等级为2时直接`swh(10,20)`).
-- 比如`sf.spad(10,20,30,40)`就获得内间距上10,右20,下30,左40的样式`{paddingTop:10,paddingRight:20,paddingBottom:30,paddingLeft:40}`.(可以给出1-4个参数,根据局不同参数个数设置不同内间距,就和在css中一样的效果)

如果一个view要同时具有上面举例的这些样式属性的时候就只需要这样`<View style={[s.center,s.w,sf.swh(100,200),sf.spad(10,20,30,40)]}></View>`

如果不喜欢s,sf这两个名称的话,可以像上面一样自定义这两个变量名的名称,上面就把这两个变量名改为了styles,styleFunctions,大家可以根据自己的需要自定义名称,上面只是演示,自定义名称的话应该尽量的短小.

### 三种导入方式的优缺点:

- 0->非侵入式导入:

-- 优点:不占用任何一个全局变量,不会造成全局变量污染.IDE中可以通过s.,sf.来获得属性名,方法名,方法参数的提示
-- 缺点:每个需要用到的文件中都要import,new一次.每个样式属性名,样式方法名前面都要写s.,sf.比较麻烦.如果你自己取更长的别名的话可能使代码变长
- 1->部分侵入式导入:
-- 优点:只用在主入口文件(可能是index.js,index.ios.js,index.android.js,Main.js等等,具体看自己的情况)中import,new一次,以后就可以在项目中的所有文件使用.IDE中可以通过s.,sf.来获得属性名,方法名,方法参数的提示
-- 缺点:会占用s,sf(或者你自定义的名称)的两个全局变量.每个样式属性名,样式方法名前面都要写s.,sf.比较麻烦.
- 2->完全侵入式导入:
-- 优点:只用在主入口文件中import,new一次,以后就可以在项目中的所有文件使用.调用设置样式的方法时不用再sf.spad(15),直接spad(15).获得屏幕的宽高直接w,h,而不用s.w,s.h,预置背景色,颜色,flex布局等也是直接写.简单方便.
-- 缺点:会占用s,以及一系列s开头的方法的全局变量,以及一系列颜色,flex布局,宽高样式相关的全局变量,造成全局变量污染.IDE中不能用sf.来获得方法名,方法参数的提示,对方法名不熟悉的话也会造成一定的困扰.

## 样式属性一览表(即上面的s对象中的键值对):
为了方便大家不同的习惯,有的相同的样式属性会有不同的键名,比如让子组件水平,垂直居中的样式属性,可以`s.center`(见名知意,但略长),也可以直接`s.c`(超级短,但不了解的人根本不知道这是啥).

固定部分,下面部分的内容所有侵入式级别都是通过`s.center`这样的形式调用

| key(样式属性键名) | value(对应的样式属性) |解释|
| :-------------: |:-------------:|:-------------:|
|\----------------|对子组件的布局方式部分|\----------------|
|spaceB或spaceBetween或sB|{justifyContent:"space-between"}|让子组件向两边分开|
|spaceA或spaceAround或sA|{justifyContent:"space-around"}|让子组件向两边分开,并且左右留间距|
|row|{flexDirection:'row'}|让View子组件横向排布(默认纵向)|
|wrap|{flexWrap:'wrap'}|超出部分子组件换行|
|\----------------|对子组件的对齐方式部分|\----------------|
|center或c|{justifyContent:'center',alignItems:'center'}|让子组件水平,竖直都居中|
|aliCenter或aliC|{alignItems:'center'}|让子组件在次轴方向上居中对齐|
|aliEnd或aliE|{alignItems:"flex-end"}|让子组件在次轴方向上向尾部对齐|
|justCenter或justC|{justifyContent:'center'}|让子组件在主轴方向上居中对齐|
|justEnd或justE|{justifyContent:"flex-end"}|让子组件在主轴方向上向尾部对齐|
|\----------------|对自身组件相对父组件的对齐方式部分|\----------------|
|aliSelfStart或aliSS|{alignSelf:'flex-start'}|对齐父组件头部|
|aliSelfCenter或aliSC|{alignSelf:'center'}|相对父组件居中对齐|
|aliSelfEnd或aliSE|{alignSelf:'flex-end'}|对齐到父组件的尾部|
|\----------------|绝对定位,相对定位部分|\----------------|
|rel|{position:'relative'}|设置组件为相对定位|
|abs|{position:'absolute'}|设置组件为绝对定位|
|absTop或absT|{position:'absolute',top:0}|设置组件为绝对定位,并定位在父组件最顶部|
|absBottom或absB|{position:'absolute',bottom:0}|设置组件为绝对定位,并定位在父组件最底部|
|absLeft或absL|{position:'absolute',left:0}|设置组件为绝对定位,并定位在父组件左方|
|absRight或absR|{position:'absolute',right:0}|设置组件为绝对定位,并定位在父组件右方|
|\----------------|图片样式部分|\----------------|
|cover|{resizeMode:'cover'}|图片以刚好占满指定宽高的形式显示|
|contain|{resizeMode:'contain'}|图片以刚好能在指定宽高内显示完整的形式显示|
|\----------------|文字样式部分|\----------------|
|textCenter或tC|{textAlign:'center'}|文字居中对齐显示|
|textLeft或tL|{textAlign:'left'}|文字靠左对齐显示|
|textRight或tR|{textAlign:'right'}|文字靠右对齐显示|
|\----------------|宽高样式部分|\----------------|
|w或width|{width:Dimensions.get('window').width},|当前屏幕宽度|
|h或height|{height:Dimensions.get('window').height}|当前屏幕高度|
|w100|{width:'100%'}|和父组件等宽|
|h100|{height:'100%'}|和父组件等高|
|wh100|{width:'100%',height:'100%'}|和父组件等宽等高|
|wh0|{width:0,height:0}|没有尺寸的宽高样式|
|whAll|whAll:{width:w,height:isios?h:h-25}|占满整个屏幕的宽高样式|
|\----------------|其他常用样式属性部分|\----------------|
|hidden|{overflow:'hidden'},|超出组件范围内容隐藏|
|show或visible|{overflow:'visible'}|超出组件范围内容显示|
|opa0|{opacity:0},|透明度0|

多个常用样式部分,下面的样式在侵入式等级0和1下依然是`s.f3`(表示{flex:3})这样调用,在侵入式等级2下直接`f3`这样调用.

- 一系列的flex布局部分:

s.f1到s.f12分别对应{flex:1}到{flex:12}

- 一系列的由白到黑的灰色背景颜色,文字颜色:

s.bg000,s.bg111一直到s.bgeee,s.bgfff分别对应由黑{backgroundColor:'#000'}到白{backgroundColor:''#fff}的灰色背景颜色

s.c000,s.c111一直到s.ceee,s.cfff则分别对应由黑{color:'#000'}到白{color:''#fff}的灰色文字颜色

- 一系列由屏幕宽高按比例计算出来的常用宽高的值:

以屏幕宽高375,667为例.
比如h1,w1就分别为375,667.h2,w2就分别为187.5,333.5.(这里的1,2就是对对屏幕宽高的除数,预设的值有[1,2,3,4,5,6,7,8,9,10,11,12,16,32,64,128]).

`注意:这里得到的是屏幕的屏幕宽高比例算出来的数值,而非样式,使用时应该sf.sw(w12),sf.sh(h3)这样使用.`

## 样式方法一览表(即上面的sf对象中的方法)
和上面一样,相同的方法可能会给出多个对应的键名.(为了以侵入式等级2导入使用时尽量的不与全局变量冲突,所以所有方法名(键名)前面都加有一个s)

下面的所有方法,在侵入式0和1中都是通过`sf.sw(100)`方式来调用,在侵入式等级2中都是直接通过方法名调用`sw(100)`

| key(样式方法键名) | 对应的样式方法的解释 |对应的样式方法的使用范例(为方便以侵入式2为例)|
| :-------------: |:-------------:|:-------------:|
|\----------------|宽高样式方法部分|\----------------|
|sw或swidth|通过给定的宽度数值设置宽度|sw(100)设置宽度为100|
|sh或sheight|通过给定的高度数值设置高度|sh(h2)设置高度为屏幕高度的一半|
|swh|通过给定的宽度/高度数值同时设置宽度/高度(只给出一个时设置宽高为相同的数值)|swh(100,200),设置宽100,高200;swh(100),设置宽高都为100|
|\----------------|边框样式方法部分|\----------------|
|sbw或sborderWidth|设置边框的宽度为指定的值|sbw(2),设置边框宽为2|
|sbc或sborderColor|设置边框颜色为指定颜色|sbc('#f90'),设置边框颜色为橙色|
|sbr或sborderRadius|设置边框圆角为指定大小|sbr(50),设置边框大小为50|
|sb或sborder|统一设置边框的常用属性|sb('#f90',50,2),设置边框颜色为橙色,圆角为50,宽度为2.第三个参数宽度,默认值为1|
|\----------------|内间距样式方法部分|\----------------|
|spadT或spadTop|设置上内间距为指定数值|spadT(100),设置上内间距为100|
|spadB或spadBottom|设置下内间距为指定数值|和上面一样|
|spadV或spadVertical|设置竖直方向(上下)内间距为指定数值|和上面一样|
|spadL或spadLeft|设置左内间距为指定数值|和上面一样|
|spadR或spadRight|设置右内间距为指定数值|和上面一样|
|spadH或spadHorizontal|设置水平方向(左右)内间距为指定数值|和上面一样|
|spad或spadAll|上下左右方向的内间距一起设置,参数可以是1到4位,和css中一样,给出1位参数时表示上下左右内间距都设置为该值;2位时竖直方向内间距为第1位参数的值,水平方向内间距为第2位参数的值;3位参数时水平方向内间距为第2位参数的值,上下方向内间距分别为第1,3位参数的值;4位参数时第1,2,3,4为参数分别对应上右下左方向的内间距值|spad(10),上下左右内间距都为10;spad(10,20),竖直方向内间距10,水平方向20;spad(10,20,30),水平方向内间距20,上10,下30;spad(10,20,30,40),上右下左方向的内间距值分别为10,20,30,40|
|\----------------|外间距样式方法部分|\----------------|
|smarT或smarTop|设置组件距离上面的距离为指定的数值|smarT(100),设置组件距离上面距离为100|
|smarB或smarBottom|设置组件距离下面的距离为指定的数值|和上面一样|
|smarV或smarVertical|设置组件距离上下的距离为指定的数值|和上面一样|
|smarL或smarLeft|设置组件距离左面的距离为指定的数值|和上面一样|
|smarR或smarRight|设置组件距离右边的距离为指定的数值|和上面一样|
|smarH或smarHorizontal|设置组件距离左右的距离为指定的数值|和上面一样|
|smar或smarAll|上下左右的外间距一起设置|和spad方法一样|
|\----------------|绝对定位样式方法部分|\----------------|
|sabsT或sabsTop|设置组件为绝对定位,并距离父组件顶部指定高度|sabs(10),组件绝对定位,并距离父组件顶部距离10|
|sabsB或sabsBottom|设置组件为绝对定位,并距离父组件底部指定高度|同上|
|sabsL或sabsLeft|设置组件为绝对定位,并距离父组件左边指定宽度|同上|
|sabsR或sabsRight|设置组件为绝对定位,并距离父组件右边指定宽度|同上|
|sabsAll|设置组件为绝对定位,并占满整个父组件(没有参数时),可以给出1到4个参数分别表示距离父组件的上右下左的距离|sabs(10,15,20,25),设置该组件为绝对定位,并占满父组件后距离父组件上右下左距离分别为10,15,20,25|
|\----------------|相对定位样式方法部分|\----------------|
|srelT或srelTop|设置组件为相对定位,并距离自身原本位置向上挤出指定的距离|srelT(100),组件相对定位,并距离自身原本位置向上挤出距离100|
|srelB或srelBottom|设置组件为相对定位,并距离自身原本位置向下挤出指定的距离|同上|
|srelL或srelLeft|设置组件为相对定位,并距离自身原本位置向左挤出指定的距离|同上|
|srelR或srelRight|设置组件为相对定位,并距离自身原本位置向右挤出指定的距离|同上|
|\----------------|绝对定位或相对定位时设置上下左右的距离|\----------------|
|sT或sTop|上|sT(10)|
|sB或sBottom|下| |
|sL或sL|左| |
|sR或sRight|右| |
|spos或sposition|上下左右一起设置|spos({t:100,b:70,l:50,r:80}),上线左右距离分别为100,70,50,80;可以只设置其中任意多个属性spos({t:50,l:30})|
|\----------------|文字样式方法部分|\----------------|
|sc或scolor|设置文字颜色为指定颜色|sc('#f90'),设置背景色为橙色|
|sfz或sfontSize|设置文字大小为指定大小|sfz(20),设置文字大小为20|
|slh或slineHeight|设置文字行高为指定高度|slh(40),设置文字行高为40|
|sff或sfontFamily|设置文字字体为指定字体|sff('PingFangSC-Light'),设置文字字体为'PingFangSC-Light'|
|stext|统一设置文字所有常用属性,stext(大小,颜色,行高,字体,是否背景色透明(老版本文字有时会有默认背景色))|stext(20,'#fff',40,'PingFangSC-Light',true),设置字体大小20,颜色白色,行高40,字体PingFangSC-Light,背景色透明(老版本文字有时会有默认背景色),可以给出任一多个参数,没有给出的默认系统样式|
|\----------------|其他常用样式方法部分|\----------------|
|sop或sopacity|设置透明度为指定数值|sop(0.5),设置透明度为0.5|
|sbgc|设置背景色为指定的颜色|sbgc('#f90'),设置背景色为橙色|

## 拓展与进阶

### 样式的重用
可能大家会想到样式的重用性问题,可能一个样式会用到多个View上,这也是没有问题的.

在标准方法中的样式重用

```
const styles=StyleSheet.create({
    container:{
        padding:15,
        width:Dimensions.get('window').width,
        backgroundColor:'#999',
        marginTop:100,
    },
})
```

`<View style={[styles.container]}></View>`

在插件中可以这样用

`let containerStyle=[sf.spad(15),sf.sw(s.w),s.bg999,sf.smarT(100)];`

`<View style={[containerStyle]}></View>`

如果多个界面都想用到这个样式的话,在侵入式等级1,2中可以直接

`s.containerStyle=[sf.spad(15),sf.sw(s.w),s.bg999,sf.smarT(100)];`

这样在其他所有界面就可以这样使用样式了

`<View style={[s.containerStyle]}></View>`

### 添加新的自定义样式
这里我只写入了一些我常用到的样式属性和样式方法,如果有一些你用到的常用的属性或方法里面没有的话也没有关系,直接向其中加入就可以了.

在导入插件后,如果需要新增样式属性可以直接

`s.center={justifyContent:'center',alignItems:'center'}`

如果需要新增样式方法可以直接

`sf.sw=(w)=>{return {width:w}}`

或者直接在插件源码中增添,修改都是可行的方法.

## (↓ˉ▽ˉ↓)

如果大家觉得我的组件好用的话,帮到你的话,欢迎大家Star,Fork,如果有什么问题的话也可以在github中想我提出,谢谢大家的支持.

