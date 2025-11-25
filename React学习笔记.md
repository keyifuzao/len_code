# React学习笔记

## 一、组件及脚手架常识

### 1、组件常识

**当前以及未来的前端开发，一定是：组件化/模块化**

- 有利于团队协作开发
- 便于组件的复用：提高开发效率，方便后期维护、减少页面中的冗余代码

**如何划分组件**

- 业务组件：针对项目需求封装
  - 普通业务组件 没啥复用性，只是单独拆出来的一个模块
  - 通用业务组件 具备复用性
- 功能组件：适用于多个项目 例如`ui`组件库的组件
  - 通用功能组件

**因为组件化开发，必然会带来工程化的处理**，也就是基于`webpack`等工具  [`vite`/`rollup`/`turbopack`]，实现组件的合并，压缩，打包等，实现代码编译、兼容、校验等

### 2、React的工程化开发

基于`webpack`自己搭建一套工程化的打包饺子，但是这样非常的麻烦和复杂；

React官方，为我们提供了一个脚手架:`creat-react-app`

脚手架，基于它创建项目，默认就把`webpack`的打包规则已经处理好了，把项目需要的基本文件也都创建好了！！

### 3、脚手架的创建

利用`react`自己的命令创建应用

```javascript
npx create-react-app my-app //安装 my-app是项目名称
//cd my-app
npm start //启动开发环境
```

一个项目中，默认会安装

- `react`：`React`框架的核心

- `react-dom`:` React`视图渲染的核心、基于`React`构建`webapp`（`html`页面）||`react-native`：构建和渲染手机微信等`App`的

- `react-script`: 脚手架为了项目目录看起来干净一些，把`webpack`打包的规则及相关的插件都隐藏到了node_modules目录下，就是对自己打包命令的一种封装，基于它的打包，可以利用node_modules中的组件对自身进行打包。

- `web-vitals`: 性能检测工具

- `script` : "`start`"开发环境、"`build`"生产环境、"`test`"单元测试、"`eject`"暴露`webpack`规则

- `eslintConfig`: 对`webpack`中进行词法检测，功能有两个：词法错误、符合代码协议标准

- `browserslist`：基于`browserlist`规范，设置浏览器的兼容情况

  `postcss-loader` + `autoprefixer` 会给`CSS3`设置相关的前缀

  `babel-loader` 会把`ES6`编译为`ES5`

  其中`production`中的`>2%`表示，使用率超过`0.2%`的浏览器，`not dead`表示不考虑`ie`， `not op_mini all`表示不考虑欧朋浏览器

  推荐配置：

  ```javascript
  >0.2%
  last 2 versions
  not ie <= 8
  ```

  项目目录

  ​	|-`src`: 所以后续编写的代码，几乎都放在`src`下，打包的时候只对这个目录的代码进行处理

  ​		|- `index-js`

  ​	|-`publish`: 放页面模板的位置

  ​		|- `index.html`

  ​	|-`package.json`

  ​	|- `node_modules`

  ​	|- ...

### 4、脚手架的高级配置

对项目进行生产环境的构建`npm build`

对项目进行`webpack`的编译的配置`npm eject`

- 编译后增加了`config`文件夹，该文件夹下存在以下配置文件：

​	|-`webpackDevServer.config.js` : 对`webpackDevServer`的配置

​	|-`webpack.config.js`: 对默认的`webpack`打包规则的配置

​	|-`path.js`打包中需要的一些路径管理

​	|- ...

- 编译后增加了`scripts`文件夹，以后执行相关命令的执行文件

   |-`bulid.js`

   |-`start.js`

   |-`test.js`

- 编译后调整了`package.json`中的依赖项当中`"dependencies"`

​	|-...

​	|-`babel-preset-react-app`：对`@babel/preset-env`语法包的重写，目的是吧`ES6`转为`ES5`

​	|-...

​	|-`sass-loader`: 预设sass预处理器

​	|-...

- 编译后新增了`"jest"`单元测试配置
- 编译后新增了`"babel"`编译工具配置，类似于我们之前写的`babel.config.js`对`babel-loader`的额外配置



常见的配置修改

- 将sass改为less

  ```javascript
  yarn add less less-loader@8
  yarn remove sass-loader
  ```

  然后到`config`文件夹中，修改`webpack.config.js`中的配置

- 配置别名 `alias`,修改`webpack.config.js`中的配置

- 改变域名端口号

  方案一：改变域名端口号，修改`script.js`中的47行，`const HOST = process.env.HOST || '127.0.0.1'`

  方案二：还可以修改环境变量`npm install --save-dev cross-env` ，配置好之后到`package.json`中修改`“script”:{"start":"cross-env PORT=8080 node scripts/start.js"}`

- 修改浏览器兼容

  修改`package.json`中的`browserslist`列表

  `postcss-loader` + `autoprefixer` 会给`CSS3`设置相关的前缀

  `babel-loader` 会把`ES6`编译为`ES5`

  遗留问题，无法处理`ES6`内置`API`的兼容

  比如修改兼容`API`，一般情况下需要引用`@babel/polyfill`，但是react已经有了自己的`react-app-polyfill`,所以不用再去导入了，可以直接调用，调用方式实在`index.tsx`的顶部`import 'react-app-polyfill/ie9' import 'react-app-polyfill/ie11' import 'react-app-polyfill/stable'`

- 处理跨域代理

  在`src`目录下新建`setupProxy.js`，`http-proxy-middleware`是实现跨域代理的模块，先安装它

  ```javascript
  yarn add http-proxy-middleware
  ```

  然后在`setupProxy.js`中编写代码

  ```javascript
  //setupProxy.js
  const { createProxyMiddleware } = require('http-proxy-middleware');
  module.exports = function (app) {
  	app.use(
  		createProxyMiddleware('/jian',{
  			target:"http://www.jianshu.com/asimov",
  			changeOrigin:true,
  			ws:true,
  			pathRewrite:{"^/jian":""}
  		})
  	)
  }
  ```

## 二、React基础

### 1、`MVC`模式与`MVVM`模式

`React`是`web`前端框架，目前市面上比较主流的的是`React`、`Angular`、`Vue`，

- 传统思想是操作DOM思想
  - 操作DOM比较消耗性能，主要原因是，可能会导致DOM重排（回流）/重绘。
  - 操作起来也相对来讲麻烦一些
- 主流的思想为不再直接去操作DOM，而是改为“数据驱动思想”，这样开发效率高，性能也相对较好
  - 我们不会直接操作DOM
  - 我们会去操作数据，当我们修改了数据，框架会按照相关的数据，让页面重新渲染
  - 框架底层实现视图的渲染，也是基于操作DOM完成的，只不过不是操作真实DOM，而是构建了一套虚拟DOM到真实DOM的渲染体系，有效避免了DOM的重排/重绘

`React`框架采用的是`MVC`体系，`Vue`框架采用的是`MVVM`体系

`MVC`：`model`数据层 + `view`视图层  + `controller`控制层，M→V→C→M 单向流动，数据驱动视图

- 需要按照专业的语法构建视图：`React`中是基于`jsx`语法构建视图
- 构建数据层：但凡在视图中，需要动态处理的（获取需要变化的，不论是样式还是内容，都要有对应的数据模型）
- 控制层：当我们在视图中，进行某些操作的时候，都是去修改相关的数据，然后React框架会按照最新的数据，重新渲染视图，从而让用户看到最新的效果

`MVVM`：`model`数据层 + `view`视图层 + `viewModel`数据/视图监听层，双向驱动系统

- 数据驱动视图，监听数据的更新，让视图重新渲染
- 视图驱动数据，监听页面中表单元素的改变，自动去修改相关的数据

### 2、`JSX`语法及应用

**`JSX`定义**: `javaScript and xml `把`JS`和`HTML`标签混合在了一起

```jsx
import React from 'react';  //react语法核心库
import ReactDOM from 'react-dom/client'  //构建WebAPP的核心库

const root = ReactDOM.createRoot(document.getElementById('root'));  //获取页面中ID为root的容器，作为"根"容器

let text = 'JSX语法'
root.render(
	<div>{ text }</div>  //单括号引用变量，JS表达式（能跟在return后面的），数据运算、三元运算符
)//基于render方法进行渲染，并插入到id为root的容器中
```

> [!CAUTION]
>
> `root`根节点需要采用`body/html`下的额外盒子作为容器，而不能把直接用`body`作为容器
>
> `render()`函数应该接收一个节点，如果不想增加新节点，可以采用空节点`<></>`进行包裹多个节点
>
> `{ }`内的值，`number`、`string`渲染出来的都是值本身，`boolean`、`null`、`undefined`、`void 0`、`BigInt`、`Symbol`类型渲染出来都是空吗，`Object`、Date、正则表达式、函数对象渲染报错，`Array`对象把数组的每一项拿出来渲染，并不是变为字符串，中间没有逗号。
>
> `style`样式需要用对象的格式，即`<div style={color:red;fontSize:16px}></div>`
>
> `class`样式类的命名需要使用`className`代替`class`

- 命名规范：
  - 小驼峰命名法：`camelCase`
  - 大驼峰命名法：`PascalCase`
  - 短横线命名方式：`kabab-case`

```jsx
//需求一、基于数据的值，来判断元素的显示隐藏

const root = ReactDOM.createRoot(document.getElementById('root'));
let flag = true, isRun = true;

root.render(
	<>
    	/* 控制元素的display样式，不论显示还是隐藏，元素本身都渲染出来 */
    	<button style={{
            display:flag? 'block': 'none'
        }}>按钮1</button>
    	<br/>
    	/* 控制元素渲染或者不渲染 */
    	{flag? <button>按钮2</button>: null}
		<br />

		<button>{isRun ? '正在处理中...': '立即提交注册'}</button>
    </>
)

//需求二、从服务器获取了一组列表数据，循环动态绑定相关内容
const root = ReactDOM.createRoot(document.getElementById('root'));
let data = [
    {id:1,title:"欢迎大家前来学习"}，
    {id:2,title:"期望大家可以学好react"}，
    {id:3,title:"大家有问题可以随时找我"}
	]

root.render(
	<>
    	<h2 className="title">今日新闻</h2>
    	<ul className="news-box">
                {data.map((item,index) => {
            	//循环创建的元素一定设置key属性，，属性值是本次循环中的唯一值，优化DOM-DIFF             
                    return 
            	<li key={item.id}>
        			<em>{ index+1 }</em>
        			&nbsp;&nbsp;
        			<span>{ item.title }</span>
        		</li>
                })}
    	</ul>
    	//扩展需求，没有数组，就是想单独循环五次
    	{new Array(5).fill(null).map((_,index) => {
    		return <button key={ index }>按钮{ index+1 }</button>
		})}
    </>
)
```

> [!NOTE]
>
> 稀疏数组、密集数组。稀疏数组不能用迭代器方法进行迭代，不能使用`forEach`等，稀数数组想要迭代需要转化为密集数组。
>
> ```js
> let arr1 = new Array(5) //undefined
> 
> arr1.forEach(()=>console.log('ok')) //undefined
> 
> let arr2 = arr1.fill(null) //使用数组的fill方法将稀数数组转化为密集数组
> 
> arr2 //[null,null,null,null,null]
> 
> arr2.forEach(()=>console.log('ok'))  //ok ok ok ok ok
> ```

### 3、`JSX`的底层渲染机制

- 第一步，把我们编写的`JSX`语法，编译为虚拟的`DOM`对象，`virtualDOM`
  - 虚拟`DOM`对象：框架自己内部构建的一套对象体系（对象的相关成员都是`React`内部规定的），基于这些属性描述出，我们所构建视图中的`DOM`节点的相关特征。
    - 基于`babel-preset-react-app` 把`jsx`编译为`React.createElement(...)` 这种格式
    - 再把`createElement`方法执行，创建出`virtualDOM`（也称为：`JSX`元素，`JSX`对象，`ReactChild`对象）
  
  ```js
  //编译过程
  //步骤一，书写源码，源码如下
  root.render(
  	<>
      	<h2 className='title' style={styObj}>占位内容</h2>
      	<div className='box'>
          <span>{ x }</span>
          <span>{ y }</span>
  		{React.createElement('button', null, '提交')}  //‘{}’不能嵌入除数组以外的其他对象，但是有一个对象是可以直接嵌入的，那就是JSX对象，虚拟DOM对象
  	</>
  )
  //步骤二，经过config配置中插件babel-preset-react-app进行编译，以下为编译源代码，该代码输出的结果就是virtualDOM
  React.creatElement(
  	React.Fragment,  //console.log打印出来为Symbol(react,fragment)
      null,
      React.createElement(  //使用React.createElement进行创建节点元素
      	"h2"   //第一个参数为标签名，为字符串格式
          { className:"title", style: styObj },  //所有的标签prop属性，以对象方式，作为第二个参数
          "\u73E0\u5CF0\u57F9\u8BAD"   //文本内容或者新的React.createElement(...)，作为第三个参数，是当前节点的Children内容（节点）
      ),
      React.createElement(
      	"div"
          { className: "box" }
          React.createElement("span", null, x),
          React.createElement("span", null, y),        
      )
  )
  //步骤三，打印步骤二的编译结果，输出virtualDOM的对象形式(">为折叠项")
  {
       $$typeof: Symbol(react.element)
  	 key: null
  	 props: {
          children: Array(2){
              $$typeof: Symbol(react.element)
              ref: null
              key: null
              type: "标签名"
              props: {className: "类名"，style: "样式内容"，children: [{Array(2)}] //子节点}
          }
       }
  	 ref: null
  	 type: Symbol(react.fragment)
  	 _owner:null
  	>_store: { validated: false }
  	 _self: null
  	 _source: null
  	>[[Prototype]]: Object
  }
  ```
  
  ```js
  //手写React.createElement(...)的方法，创建虚拟DOM对象
  
  export function createElement(ele,props,...children){
  	let virtualDOM = {
          $$typeof: Symbol('react.element'),
          key:null,
          ref:null,
          type:null,
          props:{}
      }
      let len = children.length
      virtualDOM.type = ele
      if(prop!= null){
          virtualDOM.props = {
              ...props
          }
      }
      if (len === 1) virtualDOM.props.children = children[0];
      if (len > 1) virtualDOM.props.children = children;
      return virtralDOM
  }
  ```
  
  
  
- 第二步，把构建的`virtualDOM`渲染为真实`DOM`
  - 真实`DOM`对象：浏览器页面中，最后渲染出来，让用户看见的`DOM`元素
  
  - 基于`ReactDOM`中的`render()`方法处理的
  
    - `V16`版本中
  
    ```js
    ReactDOM.render(
    	<>...</>
    	document.getElementById('root')
    )
    ```
  
    - v18版本中
  
    ```js
    const root = ReactDOM.createRoot(document.getElementById('root'));
    root.render(
    	<>...</>
    )
    ```
  
    - 虚拟`DOM`转化为真实`DOM`机制
  
    ```js
    //封装一个对象迭代的方法
    	//基于传统的for/in循环，会存在一些弊端，性能较差，既可以迭代私有的，也可以迭代公有的，只能迭代“可枚举，非Symbol类型的属性”[1]
    	//解决思路，获取对象所有的私有属性，私有的，不论是否可枚举，不论类型，可以使用
    		//*Object.getOwnPropertyName(arr) -> 获取对象非Symbol类型的私有属性，！无关是否可以枚举
    		//*Object.getOwnPropertySymbols(arr) -> 获取Symbol类型的私有属性
    		//获取所有的私有属性Object.getOwnPropertyNames(arr).concat(Object.getOwnPropertySymbol(arr))
    		//基于ES6中的Reflect.ownKeys()方法实现等同效果
    //const each = function each(obj, callback){
    //    if(obj === null || typeof obj !== 'object') throw new TypeError('obj is not a object')
    //    if(typeof callback !== 'function') throw new TypeError('callback is not a function')
    //    let keys =Reflect.ownKeys(obj)
    //    keys.forEach(key => {
    //        let value = obj[key];
            //每一次迭代，都把回调函数执行‘
    //        callback(value, key)
    //    })
    //}
    
    //Array.prototype.BB = 200
    //let arr = [ 10,20 ]
    //arr[Symbol('AA')] = 100
    
    //each(arr, (value, key) => {
    //    console.log(value,key)
    //})
    //let keys = Object.getOwnPropertyNames(arr).concat(Object.getOwnPropertySymbol(arr))
    //等价方法: let keys = Reflect.ownKeys(arr)
    //keys.forEach( key=>{
        //console.log(key,arr[key])
    //} )
    
    export function render(virtruaDOM ,container){
        let { type,props } = virtualDOM;
        if (typeof type === 'string'){
            let ele = document.createElement(type)
            each(props, (value, key ) => {
                if(key === 'className'){
                    ele.className = value
                    return
                }
                if(key === 'style'){
                    each(value , (var,attr) => {
                        ele.style[attr] = val
                    })
                    return
                }
                if(key === 'children'){
                    let children = value;
                    if(!Array.isArray(children)) children = [children];
                    children.forEach(child => {
                        if(/^(string | number)$/.test(typeof child)){
                            ele.appendChild(document.createTextNode(child))
                            return
                        }
                        render(child,ele)
                    })
                    return
                }
                
                ele.setAttribute(key, value)
            })
            container.appendChild(ele)
        }
        
    }
    ```
  
    > [!NOTE]
    >
    > [1]一般来讲，内置的属性都是不可枚举的，枚举的意思是可以被列举的，例如被`for/in`，`object/keys`等列举出来的，自定义的属性都是可枚举的！！
    >
    > 但是我们可以设定成员的枚举性，`Object.defineProperty()`
    >
    > 为元素设置属性，有两种方式，
    >
    > 	- 1、*元素.属性=属性值* 原理，对于内置属性，是设置在元素的标签上，对于自定义属性来讲，是给对象的堆内存空间  		中新增成员，结果是不会设置在标签上，获取方式*元素.属性* ,删除方式*`delete` 元素.属性*
    > 	- 2、*元素.`setAttribute`(属性.属性值)*，原理是直接写在元素的标签上，获取:`getAttribute`，删除:`removeAttribute` 
  
- 第一次渲染页面是直接从`VirtualDOM`转化为真实`DOM`，但后期视图更新的时候，需要经过一个`DOM-DIFF`的算法进行对比，计算出补丁包`Patch`（两次视图差异的部分），把`Patch`补丁包进行渲染。  

### 4、`React`函数组件

- 创建：在`src`目录下，创建一个`XXX.jsx`的文件，就是要创建一个组件，我们在此文件中，创建一个函数，让函数返回`JSX`视图或者`JSX`元素、`virtualDOM`虚拟`DOM`对象
- 调用：基于`ES6Module`规范，导入创建的组件，可以忽略组件拓展名，然后像写标签一样调用这个组件即可。可以单闭合调用(<Component />)，也可以双闭合调用(<Component></Component>)
- 命名：组件的命名，我们一般都采用`PascalCase`(大驼峰)命名法

> [!TIP]
>
> 调用组件的时候，我们可以给调用的组件设置（传递）各种各样的属性，如果设置的属性值不是字符串格式，需要基于`{}`花括号进行嵌套

```js
//./src/views/DemoOne.jsx
const DemoOne = function DemoOne(){
	return (
		<div className= 'demo-box'>
			...
		</div>
	)
}
export default DemoOne
```

```jsx
// ./src/index.jsx

import React from 'react';
import ReactDOM from 'react-dom/client'
import DemoOne from '@views/DemoOne'

const root = ReactDOM.creatRoot(document.getElementById('root'));
root.render(
	<DemoOne title='我是标题' x={10} data={[100,200]} className='box' style={{fontSize: '20px'}} />
)
```

渲染机制

- 基于`babel-preset-react-app`把调用的组件转换为`createElement`格式

  ```jsx
  React.creatElement(DemoOne, {
  	title:"\u6211\u662F\u6806\u9898",
  	x:10,
  	data: [100,200],
  	className: 'box'm
  	style: {
  		fontSize: '20px'
  	}
  })
  ```

- 把`createElement`方法执行，创建出一个`virtualDOM`对象

  ```js
  {
  	$$typeof: Symbol(react.element),
  	key:null,
  	proprs:{
  		title:'我是标题',
  		x: 10,
  		data :[
  			100,200
  		],
  		className: 'box',
  		style: {fontSize: '20px'}
  	},
  	ref:null,
  	type: DemoOne,
  }
  ```

- 基于`root.render`把`virtualDOM`变为真实的`DOM`，`type`值不再是一个字符串，而是一个函数，此时，把函数执行 -> `DemoOne()`，把`virtualDOM`中`props`，作为实参传递给函数 -> `DemoOne(props)`，接收函数执行的返回结果，最后基于`render`把函数组件返回的虚拟`DOM`转化为真实`DOM`插入到`#root`容器中

### 5、`Props`属性

- 属性`Props`的处理

  - 调用组件，传递进来的属性是**只读**的，原理是因为props对象被冻结了`Object.freeze(obj)`

    获取：`props.xxx`

    修改: `props.xxx =  xxx`   -> 报错

    > [!NOTE]
    >
    > 关于对象的规则设置：
    >
    > ​	冻结: 冻结对象的方法`Object.freeze(obj)`检测是否被冻结`Object.isFrozen(obj)`，不能修改，新增，不能删除，不能劫持`Object.defineProperty()`
    >
    > ​	密封：密封对象的方法`Object.seal(obj)`，检测是否为密封对象`Object.isSealed(obj)`，只能修改，不能新增，不能删除，不能劫持`Object.defineProperty()`
    >
    > ​	不可扩展：扩展对象的方法`Object.preventExtensions(obj)`，检测是否可扩展`object.isExtensible(obj)`，能修改，删除，劫持`Object.defineProperty()`，不能新增
    >
    > ```js
    > let obj = {
    > 	x: 10,
    > 	y: 20
    > }
    > Object.freeze(obj) //冻结对象
    > console.log(Object.isFrozen(obj) -> true/false)
    > Object.seal(obj) //密封对象
    > console.log(Object.isSealed(obj) -> true/false)
    > Object.preventExtensions(obj) //扩展对象
    > console.log(object.isExtensible(obj) -> true/false)
    > //obj.x = 100;
    > //obj.z = 300;
    > //delete obj.y;
    > console.log(obj)
    > ```

  - 向子组件传递参数

    - 父组件(`index.jsx`)调用子组件(`DemoOne.jsx`)的时候，可以基于属性，把不同的信息传递给子组件，子组件接收响应的属性值，呈现出不同的效果，让组件的复用性更强

    - 虽然对于传进来的属性不能修改，但是可以去规则校验

      给属性可以设置默认值。可以使用`函数组件.defaultProps = { 键:值}`的方法进行

      设置其他规则，例如数据值格式，是否必传...*需要使用官方的一个插件`prop-types`* 传递进来的属性，首先会经历规则的校验，不管校验成功还是失败，最后都会把属性给props，只不过如果不符合设定的规则，控制台会抛出警告错误

    - 如果一定要修改属性值，我们可以把props中某个属性值赋值给新的变量，然后修改变量，赋值给组件中的其他地方

    > [!NOTE]
    >
    > ```jsx
    > //./src/views/DemoOne.jsx
    > const DemoOne = function DemoOne(props){
    >     let {className, style, title} =props
    > 	return (
    > 		<div className= {`demo-box ${className}`} style={style}>
    >             <h2 className=" title ">{ title }</h2>
    > 		</div>
    > 	)
    > }
    > export default DemoOne;
    > 
    > //设置默认值
    > //通过把函数当作对象，设置静态的私有属性方法，来给其设置属性的规则
    > DemoOne.defaultProps = {
    >     x:0,
    >     
    > }
    > //使用插件prop-types进行规则校验
    > import PropTypes from 'prop-types'
    > DemoOne.propTypes = {
    >     title:PropTypes.string.isRequired，//类型为字符串，必传
    >     x:PropTypes.number//类型为数字，无必传要求
    >     y:PropTypes.oneOfType({ //类型对多个
    >     	PropTypes.number,
    >     	PropTypes.bool
    > 	})
    > }
    > ```
    >
    > 

### 6、插槽机制

基于传递属性的方式把相关信息传递给子组件，假如组件底部“可能”有按钮，根据需求判断需要还是不需要：

- 解决1，我们可以需要的按钮，再子组件中写死，后期基于传递进来的属性，判断按钮的显示隐藏

- 解决2，我们也可以把按钮的区域预留出来，但是内容不写，内容让调用组件的时候，把东西传进来

  -> 基于`props.children`获取传递的子节点信息，封装组件的时候预留插槽位置，里面的内容不用写，基于双闭合调用方式，把插槽信息，传递给组件，组件内部进行渲染即可，

  -> 和属性一样，都是想办法，让组件具备更强的复用性，传递数据值用属性，传递HTML结构，用插槽。

**默认插槽**

```jsx
// ./src/index.jsx

import React from 'react';
import ReactDOM from 'react-dom/client'
import DemoOne from '@views/DemoOne'

const root = ReactDOM.creatRoot(document.getElementById('root'));
root.render(
	<>
    	<DemoOne title="react标题一" x={10}>
        	<span>字符串A</span>
            <span>字符串B</span>
    	</DemoOne>
    
    	<DemoOne title="react标题二">
        	<span>字符串C</span>
    	</DemoOne>
    
    	<DemoOne title="react标题三" />
    </>
)
```



```jsx
//./src/views/DemoOne.jsx
import PropTypes from 'Prop-types'
import React from 'react'

const DemoOne = function DemoOne(props){
	let { title,x,children } = props;
	//要对children的类型做处理
	//if(!children){
	//	children=[];
	//}else if(!Array.isArray(children)){
	//	children = [ children ]
	//}
    //可以基于React.children对象提供的方法，对props.children做处理：count/forEach/map/toArray...
    //好处：在这些方法的内部，已经对children的各种形式做了处理
	children = React.Children.toArray(children)
    
	return (
        <div className="demo-box">
            {children[0]}
            <br/>

            <h2 className="title">{ title }</h2>
            <span>{ x }</span>

            <br/>
            {children[1]}
		</div>
	)
}
export default DemoOme
```

**具名插槽**

```jsx
// ./src/index.jsx

import React from 'react';
import ReactDOM from 'react-dom/client'
import DemoOne from '@views/DemoOne'

const root = ReactDOM.creatRoot(document.getElementById('root'));
root.render(
	<>
    	<DemoOne title="react标题一" x={10}>
        //1、这里指定slot的属性名字
        	<span slot='header'>字符串A</span>
            <span slot='footer'>字符串B</span>
        	<span>字符串C</span>
    	</DemoOne>
    </>
)
```

```jsx
//./src/views/DemoOne.jsx
import PropTypes from 'Prop-types'
import React from 'react'

const DemoOne = function DemoOne(props){
    //**这里传进来的children是已经被react编译成react-virturaDOM对象了，而不是HTML标签
	let { title,x,children } = props;
    //2、通过React.children自带的方法把子节点转化为列表结构
	children = React.Children.toArray(children)
    //3、创建插槽容器，接收分类列表
    let headerSlot = [], 
        footerSlot = [],
        defaultSlot = [];
    //4、对children进行筛选，装入3中的容器
    children.filter(child => {
        //5、将slot属性通过子列表的props属性解构出来
        let {slot} = child.props
        if (slot === 'header'){
            headerSlot.push(child)
        }else if(slot === 'footer'){
            footerSlot.push(child)
        }else {
            defaultSlot.push(child)
        }
        
    })
    
	return (
        <div className="demo-box">
            {headerSlot}
            <br/>

            <h2 className="title">{ title }</h2>
            <span>{ x }</span>
            {defaultSlot}

            <br/>
            {footerSlot}
		</div>
	)
}
export default DemoOme
```

### 7、封装组件/函数和类组件

```jsx
// ./src/index.jsx
import React from 'react';
import ReactDOM from 'react-dom/client'
import Dialog from './components/Dialog'

const root = ReactDOM.creatRoot(document.getElementById('root'));
root.render(
	<>
    	<Dialog title='友情提示' content='大家出门做好个人防护'/>
    	<Dialog content="我们一定要好好学React！！">
        	<button>确定</button>
        	<button>不确定</button>
    	</Dialog>
    </>
)

```



```jsx
// ./src/components/Dialog.jsx
import PropType from 'Prop-types'
import React from 'react'
const Dialog = function Dialog(props){
    let { title, content, children}
    children = React.children.toArray(children)
    return(
    	<div className="dialog-box">
            <div className="header" >
                <h2 className='title'>{ title }</h2>
                <span>x</span>
            </div>
            <div className="main">
                { content }
            </div>
            {children.length>0?<div className= "main">{ children }</div>: null}
        </div>
    )
}
// 属性校验规则
//设置默认值
Dialog.defaultProps = {
    title:'温馨提示'
}
//设置类型
Dialog.proTypes= {
    title.PropTypes.string,
    content.PropTypes.string.isRequired
}
export default Dialog
```

- 静态组件（**函数组件**）

  ​	函数组件是静态组件，第一渲染组件，把函数执行，产生一个私有的上下文，把解析出来的`props`（含`children`）传递进来，但是冻结了，对函数返回的`JSX`元素(`virtualDOM`)进行渲染。

  ​	当我们点击按钮的时候，会把绑定的小函数执行，执行后会修改上级上下文中的变量，私有变化值发生了变化，但是视图不会更新，函数组件第一次渲染完毕后，组件中的内容，不会根据组件内的某些操作，在进行更新，**除非在父组件中，重新调用这个函数组件**。

  ```jsx
  // ./src/index.jsx
  
  import React from 'react';
  import ReactDOM from 'react-dom/client'
  import Vote from './views/Vote'
  
  const root = ReactDOM.creatRoot(document.getElementById('root'));
  root.render(
  	<>
  		<Vote title="react其实还是很好学的！"/>
      </>
  )
  ```

  

  ```jsx
  //./src/views/Vote.jsx
  
  const Vote =function Vote(props){
      let { title } = props
      let supNum = 10
      let oppNum = 5
      return(
      	<div className="vote-box">
              <div className="header">
                  <h2 className="title">{ title }</h2>
                  <span>{ supNum + oppNum }</span>
              </div>
              <div className="main">
                  <p>支持人数：{ supNum }人</p>
                  <p>反对人数：{ oppNum }人</p>
              </div>
              <div className="footer">
                  <button onClick={()=>supNum++}>支持</button>
                  <button onClick={()=>oppNum++}>不支持</button>
              </div>
          </div>
      )
  }
  
  export default Vote
  ```

  

- 动态组件（**类组件**）

  ​	真实项目中，有这样的需求，第一次渲染后就不会在变化了，可以使用**函数组件**，但是大部分需求，都需要在第一次渲染完毕后，基于组件内部的某些操作，让组件可以更新，以此呈现出不同的效果！，这就是**动态组件**（类组件或者Hooks组件）

  ​	我们需要“类组件”来构建动态组件

  ```jsx
  // ./src/index.jsx
  
  import React from 'react';
  import ReactDOM from 'react-dom/client'
  import Vote from './views/Vote'
  
  const root = ReactDOM.creatRoot(document.getElementById('root'));
  root.render(
  	<>
  		<Vote title="react其实还是很好学的！"/>
      </>
  )
  ```

  ```jsx
  //./src/views/Vote.jsx
  //创建类组件
  	//创建一个构造函数（类），要求必须继承React.Component/PuerComponent这个类，我们习惯用ES6的class类继承，必须给当前类设置一个render的方法，放在其原型上：在render方法上，返回需要渲染的视图。
  
  import React, { Component, PureComponent } from 'react'
  //这里的{}并不是解构赋值，而是React中确切的导出的函数，可以通过{}拿到
  
  //es5继承组合式模式
  //function example(){
  //	React.Component.call(this)
  //	this.state = { x:10, y:20 }
  //}
  //Object.setProtoTypeOf(example.prototype, React.Component.prototype)
  //example.prototype.sum = function () {}
  //es6继承模式
  class Vote extends React.compontent {
      render(){
          return(
      	<div className="vote-box">
              <div className="header">
                  <h2 className="title">标题</h2>
                  <span>15人</span>
              </div>
              <div className="main">
                  <p>支持人数：10人</p>
                  <p>反对人数：15人</p>
              </div>
              <div className="footer">
                  <button onClick={}>支持</button>
                  <button onClick={}>不支持</button>
              </div>
          </div>
      )
      }
  }
  export default Vote
  ```


### 8、`ES6`类语法回顾

```js
class Parent {
    //constructor执行的是new 实例的构造函数(需要接受传递进来的实参信息，才需要设置constructor)
    constructor(x,y){
        //this 就是指向的就是创建的实例
        console.log(x,y)
        this.total = x + y
        this.getNum()
    }
    num = 200 //等价于 this.num =2000 给实例的私有属性
    getNum (){
        //这样是给Parent.prototype添加公共方法，且不可枚举
    }
    getNum = function(){
		//这样是给实例增加私有属性
    }
    getNum = () => {
        //箭头函数没有自己的this，所用到的this是宿主环境中的
        //箭头函数的this，指向的是实例 Parent
    }
    static average(){
        //把构造函数当作一个普通对象，为其设置静态的私有属性方法，外部可以直接调用Parent.average()，不需要通过原型，不可枚举
    }
    
    static avg = 1000
    	//把属性当作私有属性
}

Parent.prototype.y = 2000; //在外部手动给构造函数原生上设置公共的属性
let p = new Parent(10,20)

============================================================================================================

class Parent extent React.Component{
    //基于extend 实现继承
    //首先基于call继承，React.Component.call(this) //this ->  Parent类的实例p
    	//function Component(props, context, updater){...}
    	//给创建的实例p设置四个私有属性：props/context/refs/updater
    //其次再基于原型继承，Parent.prototype._proto_ === React.Component.prototype
    	//实例 -> Parent.prototype -> React.Component.prototype -> Object.prototype
    	//实例除了具备Parent.prototype的方法，还具备React.Component.prototype原型上提供的方法
    //最后只要自己设置了constructor，则内部第一句一定要执行super()
    constructor(n,m){
        super()//等价于React.Component.call(this)
    }
    x = 100
    get(){
    }
}

let p = new Parent(10,20)
console.log(p)
```

### 9、动静组件/组件更新机制

组件函数是静态组件：

​	组件第一次渲染完毕后，无法基于内部的某些操作让组件更新，但是，如果调用它的父组件更新了，那么相关的子组件也一定会更新，可能传递了最新的属性值进来，函数组件只具备属性，其他状态几乎没有，函数组件的优势，比类组件处理的机制简单，这样导致函数组件的渲染速度比类组件更快！

组件函数是动态组件：

​	组件在第一次渲染之后，除了父组件触发更新以外，我们可以通过`this.setState`修改状态或者`this.forceUpdate`等方式，让组件实现自更新，类组件具备：属性、状态、周期函数、ref...几乎组件应该有的东西它都具备，类组件比函数组件渲染速度慢，但是功能强大

真实项目中，我们需要根据需求，选择对应的组件

```jsx
// ./src/index.jsx

import React from 'react';
import ReactDOM from 'react-dom/client'
import Vote from './views/Vote'

const root = ReactDOM.creatRoot(document.getElementById('root'));
root.render(
	<>
		<Vote title="react其实还是很好学的！"/>
    </>
)

//相当于执行：React.createElement(Vote, {title: 'React其实还是很好学的！'})；
//执行后返回结构：
//{
//    $$type:type,
//    props: {
//        title: 'React其实还是很好学的！'
//    }
//}
```

> [!IMPORTANT]
>
> 编译逻辑：
>
> `render()`函数在渲染的时候，如果`type`是
>
> - 字符串：创建一个标签
>
> - 普通函数：把函数执行，并且把`props`传递给函数
>
> - 构造函数：把构造函数基于`new`执行，创建一个实例，然后把解析出来的`props`传递过去
>
>   例如：`new Vote({title: 'React其实还是很好学的！'})`
>
>   - 每调用一次类组件，都是创建一个单独的实例，实例与实例之间不互相影响
>   - 把类组件中编写的`render()`函数执行，把返回的`jsx`（`virtualDOM`）当作试图进行渲染
>
>   

```jsx
//./src/views/Vote.jsx
import React, { Component, PureComponent } from 'react'
class Vote extends React.compontent {
    static this.defaultProps = {
        //设置规则校验
    };
	static propTypes = {
        //设置数据类型
        title:PropTypes.string.isRequired;
        num:PropTypes.number
    }
    
    //constructor(props){  //一般不构建constructor，除非传入参数需要处理
    //    super(props)
        //this.state={}//添加state对象，如果此处不加，render()函数中也会有state属性，返回值是null
    }
	//这里consturctor函数就是实例，通过props属性传入，super函数代表React.compotent函数，React.compotent函数源码是
	//function Component(props, context, updater){
    //    this.props = props
    //    this.context = context
    //    this.refs = emptyObject
    //    this.updater = updater || ReactNoopUpdateQueue
    //}
	//如果props不传入super组件，只是super组件运行了以下，并没有将props属性挂载到super里面，那么consturctor第一次运行的收就不会挂载到函数里面，如果super()里面添加了props，那么类函数第一次就可以将挂载到super函数中的props挂载到自身，从而自身可以使用属性
    state = {
        supNum: 10;
        oppNum: 5;
    }
	//**1、第一次渲染前，钩子函数启用
    UNSAFE_componentWillMount(){
        console.log('在组件第一次渲染之前调用')  //这里的钩子函数，如果不加UNSAFE标识，会抛出黄色警告，因为该方法准备被弃用，是一个不安全的钩子
    }
	//**2、第一次渲染中，render函数启用
    render(){
        let {title} = this.props
        let { supNum, oppNum} = this.state
        return(
    	<div className="vote-box">
            <div className="header">
                <h2 className="title">{title}</h2>
                <span>{ supNum + oppNum }人</span>
            </div>
            <div className="main">
                <p>支持人数：{ supNum }人</p>
                <p>反对人数：{ oppNum }人</p>
            </div>
            <div className="footer">
                <button onClick={()=>{
                        this.setState({
                            supNum++		//即修改数据，又更新视图
                        })
                    }}>支持</button>
                <button onClick={
                        this.state.oppNum++
                        this.forceUpdate()  //强制更新，
                    }>不支持</button>
            </div>
        </div>
    	)      
		//**3、第一次渲染后，钩子函数启用
        componentDidMount(){
            console.log('在组件第一次渲染结束调用')
        }
        //4、接收最新属性之前
        UNSAFE_componentWillReceiveProps(nextProps){
            console.log(this.props,nextProps)
        }
        //**5、组件函数准备启用
        shouldConmpontUpdate(nextProps,nextState){
            //nextState：存储要修改的最新状态
            //this.state:存储的还是修改前的状态
            console.log('在组件渲染前调用')
            console.log(this.state, nextState)
            
            //此周期函数需要返回true/false
            //true代表允许更新，会继续执行下一步操作，false代表不允许更新，接下来啥也不处理
            return true
        }
        //**6、组件将要更新函数钩子启用
        UNSAFE_CompontentWillUpdate(nextProps,nextState){
            //nextState：存储要修改的最新状态
            //this.state:存储的还是修改前的状态
            console.log('在组件渲染时调用')
            console.log(this.state, nextState)
            //此函数也是不安全的周期函数，将来有被移除的可能性
        }
        //**7、组件更新完毕，再一次调用渲染结束的钩子函数
        componentDidUpdate(){
            console.log('在组件更新渲染结束调用')
        }
        componentWillUnmount(){
            console.log('组件销毁之前')
        }
    }
}
export default Vote
```

> [!IMPORTANT]
>
> 从调用类组件 `new Vote({...})`开始，类组件内部发生的事情
>
> - 属性的初始化
>   - 方法一：利用`constructor(props){super(props)}`通过实例接收`props`，这样做可以把传递来的属性挂载到`this`实例上
>   - 方法二：不再`constructor`上处理，或者不构建`constructor`函数，在`constructor`处理完毕后，内部也会把传递的`props`挂载到实例上,所以在其他函数上，只要保证`this`是实例，就可以基于`this.props`获取传递的属性，需要注意，`this.props`获得的属性对象是冻结的，只读属性
>   
> - 设置规则校验
>
> - 初始化状态
>
>   - 状态：后期修改状态，可以触发视图的更新，需要手动初始化，如果我们没有去做相关的处理，则默认会往实例挂载一个`state`，初始值为`state`，初始值为`null`  --->` this.state = null`
>
>   - 想让视图更新，我们需要基于`React.Component.prototype`提供的方法操作，
>
>     （1）`setState`方法，`this.setState(partialState)`，`partialState`的意思是**部分状态**，该方法既可以修改状态，也可以更新视图。
>
>     （2）`this.forceUpdate()`方法， 强制更新。
>
> - 触发周期函数：`componentwillMount` 周期函数（钩子函数）,组件第一次渲染之前，此周期函数，目前是不安全的，虽然现在可以用，但是未来可能会移除，所以不建议使用，使用会抛出黄色警告（可以使用`UNSAFE_componentWillMount`提前声明是不安全的，此时黄色警告会消失）。如果在`React.StrictMode`标签内嵌入组件函数，函数中有`componentwillMount`或者`UNSAFE_componentWillMount`钩子，则会发出红色警告
>
>   > `React.StrictMode`VS“`use strict`”
>   >
>   > `use strict`是`JS`的严格模式
>   >
>   > `React.StrictMode`是`React`中的严格模式，会检查`React`中不规范的语法
>
> - 触发`render()`函数
>
> - 再触发：`componentDidMount`周期函数，该函数再组件第一次渲染完毕调用，该函数执行完，页面中已经创建了真实DOM，可以进行DOM操作

> 第一次渲染的逻辑
>
> 开始 >>> `getDefaultProps` >>> `getInitialState` >>> `componentWillMount` >>> `render` >>> `componentDidMount`  >>>接组件更新状态`State`改变
>
> 组件更新
>
> 第一次渲染 `componentDidMount` >>>运行中>>> 状态`State`改变 >>> `shouldComponentUpdate()` （如果是`false`，返回运行中）>>>`componentWillUpdate()` >>> `componentDidUpdate()`（返回运行中）
>
> 第一次渲染 `componentDidMount` >>>运行中>>>属性`Prop`改变 >>> `componentWillReceiveProps()`>>>`shouldComponentUpdate()`
>
> 组件卸载
>
> 运行中>>>`componentWillUnmount()`>>>结束



- 组件更新的逻辑，修改组件内部的状态，组件会更新

  当修改了相关的状态，组件会更新

  - 触发周期函数`shouldComponentUpdate()` 代表是否要，进行组件更新

    ```js
    shouldConmpontUpdate(nextProps,nextState){
        //nextState：存储要修改的最新状态
        //this.state:存储的还是修改前的状态
        console.log('在组件渲染前调用')
        console.log(this.state, nextState)
    
        //此周期函数需要返回true/false
        //true代表允许更新，会继续执行下一步操作，false代表不允许更新，接下来啥也不处理
        return true
    }
    ```

  - 接下来继续触发周期函数`CompontentWillUpdate(nextProps,nextState)`，更新之前

    此周期函数也是不安全的，在这个阶段状态还没有修改

  - 修改状态值/属性值（`this.state.xxx`改为最新的值）

  - 触发 `render` 周期函数：组件更新

    按照最新的状态或属性，把返回的`JS`编译为`virtualDOM`

    然后和第一次渲染出来的`virtualDOM`进行`DOM-Diff`对比

    把差异的部分进行渲染

  - 触发 `componentDidUpdate()`周期函数： 组件更新完毕

  > [!NOTE]
  >
  > 如果我们是基于`this.forceUpdate()`强制更新视图，会跳过`shouldComponentUpdate `周期函数的校验，直接从`componentWillUpdate`开始更新（视图一定会更新）

> 深度优先原则：父组件在操作中，遇到子组件，一定是把子组件处理完，父组件才能继续处理
>
> 父组件的第一次渲染：
>
> 父`componentWillMount()`  >>>  父`render`({  子`componentWillMount()` >>> 子`render()` >>> 子`componentDidMount()`  }) >>>父`componentDidMount()`
>
> 父组件更新：
>
> 父`shouldComponentUpdate()` >>> 父`componentWillUpdate()` >>> 父`render`({子`componentWillReceiveProps()` >>> 子`shouldComponentUpdate()` >>> 子`componentWillUpdate()` >>> 子`render({})` >>> 子`componentDidUpdate()` }) >>> 父`componentDidUpdate()`
>
> 父组件销毁：
>
> 父`componentWillUnmount()`>>>子`componentWillUnmount()`>>>子组件销毁>>>父组件销毁

- 组件更新的逻辑，父组件更新，触发的子组件更新

  - 触发`componentWillReceiveProps()`周期函数，接收最新属性之前

    ```js
    UNSAFE_componentWillReceiveProps(nextProps){
    	//this.props:存储之前的属性
    	//nextProps:传递进来的最新属性
    	console.log(this.props,nextProps)
    }
    ```

    这个周期函数是不安全的，该周期函数完毕后，下一个钩子函数为`shouldComponentUpdate()`

### 10、`PureComponent`组件

```jsx
// ./src/index.jsx

import React from 'react';
import ReactDOM from 'react-dom/client'
import Demo from './views/Demo'

const root = ReactDOM.creatRoot(document.getElementById('root'));
root.render(
	<>
		<Demo />
    </>
)
```

`React.Component`VS`React.PureComponent`

```jsx
//./src/view/Demo.jsx
import React from 'react'

//class Demo extends React.Component {  //这里使用React.PureComponent，视图不会更新
class Demo extends React.PureComponent
    state = {
        arr:[10, 20, 30]
    }
    render(){
        let { arr } = this.state
        return (
        	<div>
                {arr.map((item,index)=>{
                    return(
                        <span key={ index } style={{
                                display:'inline-block',
                                width:100,
                                height:100,
                                background:'pink',
                                marginRight: 10
                            }}>{ item }</span>
                    )
                })}
                <br />
                <button onClick={()=>{
                        arr.push(40)
                        console.log(arr)  //[10,20,30,40]
                        this.setState({
                            arr: [...arr]//我们让arr状态值改为一个新数组（堆地址不一样），堆地址一样，不能跳过shouldCompontentUpdate,从而无法更新制图，只有把arr变成一个地址不一样的数组，才能驱动视图更新。
                        })
                        //this.forceUpdate()//pureCompontent组件可以采用这个方法跳过shouldCompontentUpdate的检查
                    }}>新增Span</button>
            </div>
        )
    }
	//这个是PureComponent添加的shouldComponetUpdate的比较原理
	//shouldComponentUpdate(nextProps, nextState) {
        //let { props, state } = this;
        //props/state: 修改之前的属性状态
        //nextProps/nextState: 修改之后的属性状态
        //return !shallowEqual(props,nextProps) || !shallowEqual(state,nextState)
    //}
}

export default Demo

```

> [!IMPORTANT]
>
> `React.PureComponent`和`React.Component`的区别
>
> `React.PureComponent`会给组件默认加一个`shouldComponentUpdate()`函数，在此周期函数中，它对新老的属性和状态，会做一个浅比较，如果经过浅比较，发现属性和状态没有改变，则返回`false`，也就是不继续更新组件，有变化才去更新！
>
> ```js
> //浅比较
> let obj = { z: 20 } //obj -> 0x001
> let objA = { x: 10; y: obj;//y -> 0x001
>             arr: [10, 20, 30] 
> 			}//arr -> 0x002
> obj.n = 1000
> let objB = { x: 10; y: obj;//y -> 0x001 ，此时堆内存中多了n:1000,objB和objA虽然内容不一样，但是堆地址相同
>             arr: [10, 20, 30] 
> 			}//arr -> 0x003
> //浅比较只比较对象的第一集，对于深层次的内容，不会再进行比较
> //迭代objA      迭代objB       对比两个属性值
> //x -> 10       x-> 10         一样
> //y -> 0x001    y-> 0x001      一样
> //arr -> 0x002  arr -> 0x003   不一样（即便两个数组长的一样，但堆中地址不一样）
>             
> //先比较对象长度，如果长度不一样，那么两个对象肯定不一致！
> //如果对象长度相等，再进行迭代比较
>     
> //利用isObject方法判断是否为对象
> const isObject = function isObject(obj) {
>     return obj !==null && /^(object|function$/.test(typeof obj))
> }
> //浅对比的方法
> const shallowEqual = function shallowEqual(objA,objB){
>     if( !isObject(objA) || !isObject(objB) ) return false;
>     if( objA === objB ) return true;
>    	//先比较长度
>     let keysA =Reflect.ownKeys(objA);
>     let keysB = Reflect.ownKeys(objB);
>     if (keysA.length !== keysB.length) return false;
>     //数量一致，再逐一比较内部的属性
>     for(let i=0 ; i<keysA.length; i++){
>         let key = keysA[i]
>         //如果一个对象中有这个成员，一个对象中没有，或者，都有这个成员，但成员值不一样，都应该判断为不相同
>         if（objB.hasOwnProperty(key) || !Object(objA[key],objB[key])）return false
>     }
>     
> }
> //Object.is(NaN,NaN) //true
> //NaN === NaN  //false
> //hasOwnProperty() 可枚举属性存在检查
> ```

### 11、Ref的相关操作
