
# React Native 每日一学(Learn a little every day)  

汇聚知识，分享精华。  
>如果你是一名React Native爱好者，或者有一颗热爱钻研新技术的心，喜欢分享技术干货、项目经验、以及你在React Naive学习研究或实践中的一些经验心得等等，欢迎投稿《React Native 每日一学》栏目。   
如果你是一名Android、iOS、或前端开发人员，有者一颗积极进取的心，欢迎关注《React Native 每日一学》。本栏目汇聚React Native开发的技巧，知识点，经验等。  

## 列表  
1. [D1:React Native 读取本地的json文件 (2016-8-18)](#d1react-native-读取本地的json文件-2016-8-18)
2. [D2:React Native import 文件的小技巧 (2016-8-19)](#d2react-native-import-文件的小技巧-2016-8-19)
3. [D3:React Native 真机调试 (2016-8-22)](#d3react-native-真机调试-2016-8-22)
4. [D4:React Native 函数的绑定 (2016-8-23)](#d4react-native-函数的绑定-2016-8-23)
5. [D5:React Native setNativeProps使用 (2016-8-24)](#d5react-native-setnativeprops使用2016-8-24)
6. [D6:ref属性不只是string（2016-8-25)](#d6ref属性不只是string2016-8-25)
7. [D7:解构赋值（Destructuring assignment）(2016-8-26)](#d7解构赋值destructuring-assignment2016-8-26)

```
模板：   
D1:标题 (日期)
------
概述
### 子标题
内容  
### 子标题
内容   
另外：记得在列表中添加链接 
```

D7:解构赋值（[Destructuring assignment][0]）(2016-8-26)
------
解构赋值语法是JavaScript的一种表达式，可以方便的从数组或者对象中快速提取值赋给定义的变量。

### 获取数组中的值
从数组中获取值并赋值到变量中，变量的顺序与数组中对象顺序对应。

```
var foo = ["one", "two", "three", "four"];

var [one, two, three] = foo;
console.log(one); // "one"
console.log(two); // "two"
console.log(three); // "three"

//如果你要忽略某些值，你可以按照下面的写法获取你想要的值
var [first, , , last] = foo;
console.log(first); // "one"
console.log(last); // "four"

//你也可以这样写
var a, b; //先声明变量

[a, b] = [1, 2];
console.log(a); // 1
console.log(b); // 2
```

如果没有从数组中的获取到值，你可以为变量设置一个默认值。

```
var a, b;

[a=5, b=7] = [1];
console.log(a); // 1
console.log(b); // 7
```

通过解构赋值可以方便的交换两个变量的值。

```
var a = 1;
var b = 3;

[a, b] = [b, a];
console.log(a); // 3
console.log(b); // 1

```

### 获取对象的值
从对象中获取对象属性的值，在声明变量的时候要与对象的属性名保持一致。

```
var o = {p: 42, q: true};
var {p, q} = o;

console.log(p); // 42
console.log(q); // true

//你也可以这样写
var a, b;

({a, b} = {a:1, b:2});

console.log(a); // 1
console.log(b); // 2
```

可以从一个对象中获取对象属性的值并赋值给与对象属性名不同的变量。

```
var o = {p: 42, q: true};
var {p: foo, q: bar} = o;
 
console.log(foo); // 42 
console.log(bar); // true  
```
和获取数组中的值一样，从对象中获取属性的值也可以设置一个默认值。

```
var {a=10, b=5} = {a: 3};

console.log(a); // 3
console.log(b); // 5
``` 



D6:ref属性不只是string（2016-8-25)
----

ref属性不仅接受string类型的参数，而且它还接受一个function作为callback。这一特性让开发者对ref的使用更加灵活。   
     

```javascript
 render: function() {
    return (
      <TextInput
        ref={function(input) {
          if (input != null) {
            input.focus();
          }
        }} />
    );
  },
```   
在ES6中我们可以使用箭头函数来为组件的ref设置一个callback。  

```javascript
  render() {
    return <TextInput ref={(c) => this._input = c} />;
  },
  componentDidMount() {
    this._input.focus();
  },
```
需要提醒大家的是，只有在组件的render方法被调用时，ref才会被调用，组件才会返回ref。如果你在调用this.refs.xx时render方法还没被调用，那么你得到的是undefined。

>心得：ref属性在开发中使用频率很高，使用它你可以获取到任何你想要获取的组件的对象，有个这个对象你就可以灵活地做很多事情，比如：读写对象的变量，甚至调用对象的函数。  



D5:React Native setNativeProps使用（2016-8-24)
----

有时候我们需要直接改动组件并触发局部的刷新，但不使用`state`或是`props`。
`setNativeProps` 方法可以理解为web的直接修改dom。使用该方法修改 `View` 、 `Text` 等 RN自带的组件 ，则不会触发组件的 `componentWillReceiveProps` 、 `shouldComponentUpdate` 、`componentWillUpdate` 等组件生命周期中的方法。

### 使用例子
 
 ```javascript
  class MyButton extends React.Component({
	setNativeProps(nativeProps) {
	     this._root.setNativeProps({   //这里输入你要修改的组件style
	     	height:48,
	     	backgroundColor:'red'
	     });
	},
	render() {
	     return (
	     	<View ref={component => this._root = component} {...this.props} style={styles.button}>
	 	     <Text>{this.props.label}</Text>
	 	</View>
	     )
	},
  });
 ```
 
### 避免和`render`方法的冲突

如果要更新一个由`render`方法来维护的属性，则可能会碰到一些出人意料的bug。因为每一次组件重新渲染都可能引起属性变化，这样一来，之前通过`setNativeProps`所设定的值就被完全忽略和覆盖掉了。


D4:React Native 函数的绑定 (2016-8-23)
----
在ES6的class中函数不再被自动绑定，你需要手动去绑定它们。

第一种在构造函数里绑定。

	  constructor(props) {
    	super(props); 
    	// Set up initial state
    	this.state = {
      		text: props.initialValue || 'placeholder' 
    };
        // Functions must be bound manually with ES6 classes
    	this.handleChange = this.handleChange.bind(this); 
另一种方式就是在你使用的地方通过内联来绑定:

	// Use `.bind`:
	 render() { 
    	return (
     	 <input onChange={this.handleChange.bind(this)}
       	 value={this.state.text} /> 
    );
	}
	// Use an arrow function:
	render() {
		 return (
			<input onChange={() => 	this.handleChange()} 
      	value={this.state.text} />
	);
以上任意一种都可以，然而在效率上却不行了。每一次调用render(可以说是非常频繁！)一个新的函数都会被创建。与在构造函数里只绑定一次相比就慢一些。

最终的选择是使用箭头函数直接替换函数在类中的声明，像这样：

	// the normal way
	// requires binding elsewhere
	handleChange(event) {
	  	this.setState({
    	text: event.target.value
		});
	}
	// the ES7 way
	// all done, no binding required
	handleChange = (event) => { 
	  this.setState({
    text: event.target.value
	  });
	}
通过这种方式，你不需要绑定任何东西。这都已经通过神奇的箭头函数被搞定了。像期望的那样，函数内部的this将会指向组件实例。
参考：[http://www.jianshu.com/p/a4c23654932e](http://www.jianshu.com/p/a4c23654932e)

D3:React Native 真机调试 (2016-8-22)
------ 
开发中真机调试是必不可少的,有些功能和问题模拟器是无法重现的,所以就需要配合真机测试,接下来就说下安卓和iOS的真机调试,不难,但是有很多细节需要注意

###iOS 真机调试
1. ``必须`` 保证调试用电脑的和你的设备处于相同的 ``WiFi ``网络环境中下
2. 打开Xcode,找到 AppDelegate.m 文件
3. 更改 jsCodeLocation 中的 localhost 改成你电脑的局域网IP地址
4. IP地址点击左面右上角WIFi图标,找到打开网络偏好设置,状态栏下就可以看见了
5. 在Xcode中,选择你的手机作为目标设备,Run运行就可以了
![React Native 真机调试的json文件-1](https://raw.githubusercontent.com/WT-Road/RNStudyNotes/master/React%20Native%20%E6%AF%8F%E6%97%A5%E4%B8%80%E5%AD%A6/images/D1/React%20Native%20%E7%9C%9F%E6%9C%BA%E8%B0%83%E8%AF%95%E7%9A%84json%E6%96%87%E4%BB%B6-1.png)

###Android 真机调试
1. 在 Android 设备上打开 USB debugging 并连接上电脑启动调试。

2. 在真机上运行的方法与在模拟器上运行一致，都是通过 react-native run-android 来安装并且运行你的 React Native 应用。

3. 如果不是 Android 5.0+ (API 21) ，那么就没办法通过 adb reverse 进行调试，需要通过 WiFi 来连接上你的开发者服务器
4. 让调试用电脑和你的手机必须处于相同的 WiFi 网络中下 打开震动菜单 (摇动设备)->前往 Dev Settings->选择 Debug server host for device->输入调试用电脑的局域网IP->点击 Reload JS

注:因为本人不是安卓开发,所以参考[http://my.oschina.net/imot/blog/512808](http://my.oschina.net/imot/blog/512808)

###细节
其实还是有些坑的,这里只说iOS 如开始所说,必须是同一网络下,有时电脑同时开着Wifi和插着网线,建议把网线拔掉,但是也不排除可以,没有试过,还有就是

``jsCodeLocation = [[RCTBundleURLProvider sharedSettings] jsBundleURLForBundleRoot:@"index.ios" fallbackResource:nil];``

这一句千万不能注释,需要注意的就这几点,很简单

D2:React Native import 文件的小技巧 (2016-8-19)
------  
开发中经常需要 import 其他 js 文件，如果需要同时导入一些相关的 js 文件时，可以创建一个索引文件方便引用。  

### 第一步：创建index.js   
在 index.js 中 import 相关的 js 文件

```
'use strict';

import * as Type from './network/EnvironmentConst';
import Request from './network/RequestManager';
import AppContext from './network/AppContext';
import ApiServiceFactory from './network/ApiServiceFactory';

module.exports = {
    ApiServiceFactory,
    Type,
    Request,
    AppContext
};
```

### 第二步：使用   
如果需要使用这些类，只需要导入index文件就可以了~

```
import {Request, ApiServiceFactory, AppContext, Type} from '../expand/index';
```


D1:React Native 读取本地的json文件 (2016-8-18)
------  
自 React Native 0.4.3，你可以以导入的形式，来读取本地的json文件，导入的文件可以作为一个js对象使用。      

### 第一步：导入json文件   

```javascript
var langsData = require('../../../res/data/langs.json');
```

ES6/ES2015     

```javascript
import langsData from '../../../res/data/langs.json'
```

### 第二步：使用   
如果`langs.json`的路径正确切没有格式错误，那么现在你可以操作`langsData`对象了。  

### Usage  

**读取`langs.json`**  

![React Native 读取本地的json文件-1](https://raw.githubusercontent.com/crazycodeboy/RNStudyNotes/master/React%20Native%20%E6%AF%8F%E6%97%A5%E4%B8%80%E5%AD%A6/images/D1/React%20Native%20%E8%AF%BB%E5%8F%96%E6%9C%AC%E5%9C%B0%E7%9A%84json%E6%96%87%E4%BB%B6-1.png)

**使用`langs.json`**    

![React Native 读取本地的json文件-2](https://raw.githubusercontent.com/crazycodeboy/RNStudyNotes/master/React%20Native%20%E6%AF%8F%E6%97%A5%E4%B8%80%E5%AD%A6/images/D1/React%20Native%20%E8%AF%BB%E5%8F%96%E6%9C%AC%E5%9C%B0%E7%9A%84json%E6%96%87%E4%BB%B6-2.png)  

@[How to fetch data from local JSON file on react native?](http://stackoverflow.com/questions/29452822/how-to-fetch-data-from-local-json-file-on-react-native)





[0]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment

