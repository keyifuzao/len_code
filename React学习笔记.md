#  React学习笔记

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

### 11、`Ref`的相关操作

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



```jsx
//./src/view/Demo.jsx
import React from 'react'

class Demo extends React.Component{
	box3 = React.createRef();//this.box3 = xxx 往实例上添加私有属性

    render(){
		return (
        	<div>
                <h2 className="title" ref='Box1'>温馨提示</h2>
                <h2 className="title" ref={ x => this.box2 }>友情提示</h2>
                <h2 className="title" ref={ this.box3 }>郑重提示</h2>
            </div>
        )
    }
	componentDidMount(){
        //第一次渲染完毕 virturaDOM已经变为真实DOM，此时我们可以获取需要操作DOM元素
        console.log(this.refs.Box1)//不推荐这么使用
        console.log(this.box2)
        console.log(this.box3.current)
    }

}

export default Demo
```

> [!TIP]
>
> 受控组件：基于修改数据/状态，让视图更新，达到需要的效果
>
> 非受控组件：基于`ref`获取`DOM`元素，我们操作`DOM`元素，来实现需求和效果基于`ref`获取`DOM`元素的语法
>
> ​	1、给需要获取的元素设置`ref`=’`xxx`‘，后期基于`this.refs.xxx`去获取相应的DOM元素<h2 ref='titleBox'>...</h2>获取`this.refs.titleBox` ,不推荐使用，因为在`React.StrictMode`标签下容易报错
>
> ​	2、把ref属性值设置为一个函数，`ref`={ `x`=> `this.xxx = x` }，`x`是函数的形参，存储的就是当前的`DOM`元素，然后我们把获取的`DOM`元素’`x`'直接挂载到实例的某个属性上（例如： `box2`），获取`this.xxx`
>
> ​	3、基于`React.createRef()`方法创建一个`REF`对象, `this.xxx = React.createRef()`等价于 `this.xxx` = { `current :null` }，`ref `= {`REF对象`(也就是`this.xxx`)}，获取`this.xxx.current`
>
> 原理：在`render`渲染的时候，会获取`virtualDOM`的`ref`属性，如果属性值是一个字符串，则会给`this.refs`增加这样的成员，成员值就是当前的`DOM`元素，如果`ref`是一个函数，则会把函数执行，把当前`DOM`元素传递给这个函数，而在函数执行内部，我们一般都会把`DOM`元素直接挂载到实例的某个属性上，如果属性值是一个REF对象，会把DOM元素赋值给对象的current属性

```jsx
//./src/view/Demo.jsx
import React from 'react'

class child1 extends React.Component{
    state = {
        x:100,
        y:200
    }
    render(){
		return (
        	<div>
                子组件1
                <em ref = {x=>this.emBox }>100</em>
            </div>
        )
    }
}

const child2 = function child2(){
    render(){
		return (
        	<div>
                子组件2
            </div>
        )
    }
}
const child3 = React.forWardRef(function child3(props, ref){
    console.log(ref)  //我们调用child3的时候，设置ref传进来的属性值“x => this.child3 = x”
    render(){
		return (
        	<div>
                子组件3
                <button ref = { ref }>按钮</button>  
                //相当于<button ref = { x => this.child3 = x }>按钮</button>
            </div>
        )
    }
})
class Demo extends React.Component{
    render(){
		return (
        	<div>
                <Child1 ref= { x => this.child1 = x }/>
                <Child2 ref= { x => this.child2 = x }/>  //会报错，不能赋值给函数ref
                <Child3 ref= { x => this.child3 = x }/>  //针对child2的解决方案
            </div>
        )
    }
	componentDidMount(){
        console.log(this.child1)  //存储的是：子组件的实例对象
        console.log(this.child2)  //报错！函数组件不能调用ref
    }

}

export default Demo
```

> [!TIP]
>
> 给元素标签设置`ref`， 目的：获取对应的`DOM`元素
>
> 给类组件设置`ref`， 目的：获取当前调用类组件创建的实例，后续可以根据实例获取子组件的相关信息
>
> 给函数组件设置`ref`， 直接报错：Function components cannot be given refs. Attempts to access this ref will fail. 但是我们让其配合`React.forWardRef` 实现`ref`的转发，目的是获取函数组件内部的某个元素

### 12、`setState`方法探索

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



```jsx
//./src/view/Demo.jsx
import React from 'react'

class Demo extends React.Component {
    state = {
        x: 10,
        y: 5,
        z: 0
    }
    handle = () => {
        //this -> 实例[宿主环境]
        let { x, y, z } = this.state; 
        this.setState({
            x:100
        },()=>{
            console.log('点击更新')
        })
    }
    shouldComponentUpdate(){  //即便返回false，setState函数也会更新，它只与点击事件关联
        return false
    }
    componentDidUpdate(){	//如果shouldcomponentUpdate返回的false，则componentDidUpdate不执行
        console.log('更新完毕')
    }
	render(){
    	console.log('视图渲染:RENDER');
        let { x, y, z } = this.state;
		return(
			<div>
                x:{x} - y:{y} - z:{z}
                <br/>
                <button onClick={this.handle}>按钮</button>
			</div>
		)
	}
}
export default Demo
```

> [!TIP]
>
> `this.setState([partialState],[callback])`
>
> `[partialState]`: 支持部分状态修改
>
> ```jsx
> this.setState({
> 	x: 100 //不论总共多少状态，我们只修改了x，其余的状态不动
> })
> ```
>
> `[callback]`: 在状态更改，视图更新完毕后触发执行
>
> ​	发生在`compontentDidUpdate()`钩子函数后，也就是这个钩子函数先运行，后才会运行`callback`，`componentDidUpdate()`会在任何状态下更改后都会触发，而回调函数方式，可以指定状态更新后处理一些事情
>
> ​	特殊情况，即便我们基于`shouldComponentUpdate`阻止了状态/视图更新，`compontentDidUpdate`周期函数不执行了，但是我们设置的这个`callback`回调函数依然会触发执行。

- 底层机制

```jsx
//./src/view/Demo.jsx
import React from 'react'

class Demo extends React.Component {
    state = {
        x: 10,
        y: 5,
        z: 0
    }
    handle = () => {
        //this -> 实例[宿主环境]
        let { x, y, z } = this.state;
		this.setState({ x:x + 1 });  
        console.log(this.state.x)  //10
        this.setState({ y:y + 1 });
        console.log(this.state.x,this.state.y) //10，5
        
        setTimeOut(() => {  //此操作为异步操作，遇到异步直接跳过，先渲染x,y，1s之后，定时器到时间后，再添加到队列中
            this.setState({ z:z + 1 }); //这里x和y是上一次更新处理的（x11，y5，z0），z还没有改，在进行渲染
            console.log(this.setState.z)
        },1000)
    }
    shouldComponentUpdate(){  //即便返回false，setState函数也会更新，它只与点击事件关联
        return false
    }
    componentDidUpdate(){	//如果shouldcomponentUpdate返回的false，则componentDidUpdate不执行
        console.log('更新完毕')
    }
	render(){
    	console.log('视图渲染:RENDER');
        let { x, y, z } = this.state;
		return(
			<div>
                x:{x} - y:{y} - z:{z}
                <br/>
                <button onClick={this.handle}>按钮</button>
			</div>
		)
	}
}
export default Demo
```

> [!TIP]
>
> `setState`在任何地方执行，都是“异步操作”
>
> ​	`react`有一套更新队列的机制（更新队列“`updater`”），基于异步操作，实现状态批处理，好处减少视图更新次数，降低渲染消耗的性能，让更新的逻辑和流程更清晰
>
> ​	进行`setState({x: x+1})`后，不会立即更新状态和视图，而是将同步的代码加入到更新的队列中，当上下文的代码都处理完毕后，会让更新队列的任务，统一渲染/更新一次“批处理”，如果有异步的，则跳过异步的指令，先渲染同步任务，等到异步完成后再调用渲染一次
>
> > 更新队列 updater
> >
> > > 任务1：修改x
> > >
> > > 任务2：修改y
> > >
> > > 任务3：修改z
>
> `setState`操作都是异步的，不论在哪执行，例如合成时间，周期函数，定时器都是异步处理，目的是实现状态的批处理，有效减少更新次数，降低性能消耗，有效管理代码的执行的逻辑顺序
>
> 原理是利用了更新队列机制
>
> > 在当前相同时间段内，浏览器此时可以处理的事情中，遇到`setState`会立即放入到更新队列中！，此时状态和视图还没有更新，当所有的代码操作结束，会刷新队列，就是通知更新队列中的任务执行，把所有放入的`setState`合并在一起，只触发一次视图更新（批处理操作）

> [!IMPORTANT]
>
> 在`React18`和`React16`中，关于`setState`是同步还是异步，是有一些区别的
>
> ​	`React18`：不论在什么地方，执行的都是`setState`，都是异步的
>
> ​	`React16`:  如果合成事件（水和事件）、周期函数，`setState`的操作是异步的，但是如果`setState`出现在其他异步操作中（如定时器，手动获取`DOM`元素绑定等），它将变为同步操作（即立即更新状态和让视图渲染）

- `flushSync`方法

```jsx
import React from 'react'
import { flushSync } from 'react-dom'
//强制同步视窗
...
handle=()=>{
	let { x, y } = this.state
	this.setState({ x:x+1 })
	console.log(this.state) //10 5 0
	flushSync(() => {
		this.setState({ y: y+1 })
		console.log(this.state) //10 5 0
	})//在flushSync操作结束后，会立即“刷新”更新队列
	console.log(this.state) //11 6 0
	this.setState({ z: this.state.x + this.state.y })
}
...

//this.setState(
//	{x: this.state+1}
//})
///////////////////////////////////////////////////
//this.setState((prevState)=>{
	//prevState: 存储之前的状态值
	//return的对象，就是我们想要修改的新状态值，支持修改部分状态
	//return {
    //  xxx:xxx
    //}
//})
```

### 13、React合成事件

> Synthetic Event
>
> 合成事件是围绕浏览器原生事件，充当跨浏览器包装器的对象，他们将不同浏览器的行为合并成一个`API`，这样做是为了确保事件在不同浏览器中显示一致的属性

- 合成事件的基本操作

  - 基础语法

  在JSX元素上，直接基于`onXxx = { 函数 }`进行事件绑定！

  浏览器标准事件，在React中大部分是支持的

  ```jsx
  //./src/view/Demo.jsx
  import React from 'react'
  
  class Demo extends React.Component {
      
      //基于React内部的处理，如果我们给合成事件绑定一个普通函数，当事件行为触发，绑定的函数执行，方法中的this会是undefined
      //解决方案：this -> 实例 基于js中的bind方法，预先处理函数中的this和实参
      //推荐方案：也可以把绑定的函数设置为“箭头函数”，让其使用上下文中的this
      //合成事件对象SyntheticBaseEvent：我们在React合成事件触发的时候，也可以获得到事件对象，只不过此对象是合成事件对象，合成事件对象中，包含了浏览器内置事件对象中的一些属性和方法，常用的都有
      //clientX/clientY
      //pageX/pageY
      //target
      //type
      //preventDefault
      //stopPropagation
      //...
      //nativeEvent:基于这个属性，可以获取浏览器内置原生的事件对象
      handle0(){  //Demo.prototype => Demo.prototype.handle = function handle()
          console.log(this) //undefined
      }
      handle1(x,y，ev){  //Demo.prototype => Demo.prototype.handle = function handle()
          //只要经过bind方法处理了，那么最后一个实参，就是传递的合成对象！！
          console.log(this,x,y,ev) //Demo实例，10，20,合成对象
      }
  	handle2= (ev) => { //实例.handle3 = () => { ... }
          console.log(this) //Demo实例
          console.log(ev) //SyntheticBaseEvent 合成事件对象（React内部经过特殊处理，把各个浏览器的事件对象统一化后构建的一个对象）
      }
      handle3 =(x,ev) ={
          console.log(x,ev)
      }
  	render(){
  		return(
  			<div>
                  <button onClick={this.handle0}>按钮</button>
                  <button onClick={this.handle1.bind(this，10，20)}>按钮</button>
                  <button onClick={this.handle2}>按钮</button>
                  <button onClick={this.handle3.bind(null,10)}>按钮</button>
  			</div>
  		)
  	}
  }
  export default Demo
  ```
  
  > [!NOTE]
  >
  > 在`js`中，是基于 `call/apply/bind` 修改`this`
  >
  > bind在react中运用
  >
  > 绑定的方法是一个普通的函数，需要改变函数中的this是实例，此时需要用到bind
  >
  > 想给函数传递指定的实参，可以基于bind预先处理，bind会把事件对象以最后一个实参传递给函数

### 14、事件及事件委托

- 前置基础

```html
<html>
    <head>
        <title>事件委托</title>
        <style>
            *{
                margin:0;
                padding:0;
            }
            html,body{
                height: 100%;
                overflow: hidden;
            }
            .center {
                position:absolute;
                top:50%；
                left:50%;
                transform: translate(-50%, -50%);
            }
            #root {
                width: 300px;
                height: 300px;
                background: lightblue;
            }
            #outer {
                width: 200px;
                height: 200px;
                background: lightgreen;
            }
            #inner {
                width: 100px;
                height: 100px;
                background: lightcoral;
            }
        </style>
    </head>
    <body>
        <div id='root' class='center'>
            <div id='outer'class='center'>
                <div id='inner'class='center'>

                </div>
            </div>
        </div>
        <script>
            //window -> document -> html -> body -> root -> outer -> inner
            const html = document.documentElement,
                  body = document.body,
                  root = document.querySeletor('#root'),
                  outer = document.querySeletor('#outer'),
                  inner = document.querySeletor('#inner');
            //root
            root.addEventListener('click', function(){
                console.log('root 捕获')
            },true)
             root.addEventListener('click', function(){
                console.log('root 冒泡')
            },false)
            //outer
            outer.addEventListener('click', function(){
                console.log('outer 捕获')
            },true)
             outer.addEventListener('click', function(){
                console.log('outer 冒泡')
            },false)
            //inner
            inner.addEventListener('click', function(){
                console.log('inner 捕获')
            },true)
            inner.addEventListener('click', function(ev){
                ev.stopPropagation()//阻止事件传播
                console.log('inner 冒泡')
            },false)
        </script>
    </body>
</html>
```

> [!NOTE]
>
> `window` ->` document` -> `html` -> `body` -> `root` -> `outer` -> `inner`
>
> 打印结果为
>
> `root` 捕获 -> `outer` 捕获 ->`inner` 捕获 ->`inner` 冒泡-> `outer` 冒泡-> `root` 冒泡
>
> 事件的传播机制，例如：当我们触发inner的点击行为的时候，第一步，从最外层向最里层逐一查找（捕获阶段，分析路径），第二部，把事件源（点击这个元素）的点击行为触发（目标阶段），第三步，按照捕获阶段的分析出来的路径，从里到外，把每一个元素的点击元素行为也触发
>
> 事件和事件绑定：事件是浏览器赋予元素的默认行为，事件绑定是给这个行为绑定一个方法。即便我们没有给`body`的点击事件绑定方法，当我们点击`body`的时候，其点击行为也会被触发，只不过啥事都没做而已
>
> `（ev）=> ev.stopPropagation() `阻止事件传播，包括冒泡和捕获阶段
>
> `(ev) => ev.stopImmediatePropagation()`也是阻止事件传播，只不过它可以当前元素绑定的其他的监听事件，如果还未执行，也不会再执行了

**事件委托就是利用事件的传播介质，实现的一套事件绑定处理方案**

> 例如：一个容器中，有很多元素都要在点击的时候做一些事情
>
> 传统方案：首先获取需要操作的元素，然后逐一做事件绑定
>
> 事件委托方法：只需要给容器做一个事件绑定，（点击内部的任何元素，根据事件的冒泡传播机制，都会让容器的点击事件触发，我们在这里，根据事件源，做不同的事情就可以了）
>
> ```js
> const body = document.body
> body.addEventListener('click', function (ev) {
> 	let target = ev.target,
> 		id = target.id
> 	if(id === 'root'){
> 		console.log('root')
> 		return
> 	}
> 	if(id === 'outer'){
> 		console.log('outer')
> 		return
> 	}
> 	if(id === 'inner'){
> 		console.log('inner')
> 		return
> 	}
>     //如果以上都不是，我们处理啥
> })
> ```
>
> 事件委托的优势：提高`JS`代码运行性能，并且把处理逻辑都集中在一起，某些需求必须基于事件委托处理，例如：除了点击XXX以外，点击其余任何东西，都....，给动态绑定元素做事件绑定
>
> 事件委托的限制：当前操作事件必须支持冒泡传播机制才可以，例如：`mouseenter`/`mouseleave`等事件是没有冒泡机制的，如果单独做的事件绑定中，做了事件传播机制的阻止，那么事件委托的操作也不会生效

### 15、React合成事件机制

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



```jsx
//./src/view/Demo.jsx
import React from 'react'

class Demo extends React.Component {

	render(){
		return(
			<div className="outer" onClick={()=>{
                        console.log('outer 冒泡')
                    }} onClickCapture={()=>{
                        console.log('outer 捕获')
                    }}>
                <div className='inner' onClick={()=>{
                        console.log('inner 冒泡')
                    }} onClickCapture={()=>{
                        console.log('inner 捕获')
                    }}></div>
			</div>
		)
	}
    compontentDidMount(){
        document.addEventListener('click', ()=>{
            console.log('document 捕获')
        }，true);
        document.addEventListener('click', ()=>{
            console.log('document 冒泡')
        }，false);
        document.body.addEventListener('click', ()=>{
            console.log('body 捕获')
        }，true);
        document.body.addEventListener('click', ()=>{
            console.log('body 冒泡')
        }，false);
        
        let root = document.queryselector('#root');
        root.addEventListener('click', ()=>{
            console.log('root 捕获')
        }，true);
        root.addEventListener('click', ()=>{
            console.log('root 捕获')
        }，false);
        
        
    }
}
export default Demo

//结果是
//document 捕获
//body 捕获
//outer 捕获
//inner 捕获
//root 捕获
//inner 冒泡
//outer 冒泡
//root 冒泡
//body 冒泡
//docment 冒泡
```

> [!NOTE]
>
> `React`中合成事件的处理原理
>
> ​	`React`的合成事件处理方式绝对不是给当前元素基于`addEventListener`做的事件绑定，React中的合成事件，都是基于“事件委托处理”的，
>
> >  在`React17`及以后版本，都是委托给`#root`这个容器（捕获和冒泡都做了委托）
> >
> > 在`React17`版本以前，都是委托给`document`容器的（而且只做了冒泡阶段的委托）
> >
> > 对于没有实现事件传播机制的事件，才是单独做的事件绑定（`onMouseEnter/onMouseLeave`这两个没有事件传播机制）
>
> 在组件渲染的时候，如果发现`JSX`元素中有 `onXxx/onXxxCapture`这样的属性，不会给当前元素直接做事件绑定，只是把绑定的方法赋值给元素的相关属性，例如：`outer.onClick = () => {console.log('outer 冒泡[合成]')}` ,注意：这不是`DOM0级`的事件绑定（`outer.onclick`）
>
> 然后对#root这个容器做了事件绑定（捕获和冒泡都做了），原因是因为组件所渲染的内容，最后都会插入到#root这个容器，这样点击页面的任何一个元素，最后都会把#root的点击行为触发，而在给#root绑定的方法中，把之前给元素设置的`onXxx/onXxxCapture`属性，在相应的阶段执行！

```html
<html>
    <head>
        <title>事件合成原理</title>
        <style>
            *{
                margin:0;
                padding:0;
            }
            html,body{
                height: 100%;
                overflow: hidden;
            }
            .center {
                position:absolute;
                top:50%；
                left:50%;
                transform: translate(-50%, -50%);
            }
            #root {
                width: 300px;
                height: 300px;
                background: lightblue;
            }
            #outer {
                width: 200px;
                height: 200px;
                background: lightgreen;
            }
            #inner {
                width: 100px;
                height: 100px;
                background: lightcoral;
            }
        </style>
    </head>
    <body>
        <div id='root' class='center'>
            <div id='outer'class='center'>
                <div id='inner'class='center'>

                </div>
            </div>
        </div>
        <script>
            //window -> document -> html -> body -> root -> outer -> inner
            const root = document.querySeletor('#root'),
                  outer = document.querySeletor('#outer'),
                  inner = document.querySeletor('#inner');
			//经过视图渲染解析，outer/inner上都有onXxx/onXxxcapture这样的属性
            
            outer.onClick = () => { console.log('outer 冒泡【合成】')}
            outer.onClickCapture = () => { console.log('outer 捕获【合成】')}
            inner.onClick = () => { console.log('inner 冒泡【合成】')}
            inner.onClickCapture = () => { console.log('outer 捕获【合成】')}
            
            //给#root做事件绑定
            root.addEventListener('click', (ev)=>{ 
            	let path = ev.path
                console.log(path) //path:[事件源 -> ... -> window] 所有祖先元素
                [...path].reverse().forEach(ele =>{
                    let handle = ele.onClickCapture;
                    if (handle) handle()
                })
            },true)
            root.addEventListener('click', (ev)=>{ 
                path.forEach(ele =>{
                    let handle = ele.onClick;
                    if (handle) handle()//在这里把绑定的合成事件方法执行，如果不经过处理，方法中的this是undefined，在执行这些方法之前，把原生的事件对象做特殊处理，返回合成事件对象，传递给函数
                })
            },false)
        </script>
    </body>
</html>
```

点击inner，按照原生的事件传播机制

`window`捕获 -> `document`捕获 -> `html`捕获 -> `body`捕获 -> `root`捕获：执行捕获阶段的委托程序 ->` outer`捕获 -> `inner`捕获

` inner`冒泡-> `outer`冒泡 ->`root`冒泡：执行冒泡阶段的委托程序->`body`冒泡->`html`冒泡 -> `document`冒泡->`window`冒泡

所谓合成事件绑定，其实并没有给元素本身做事件绑定，而是给元素设置`onXxx/onXxxCapture`这样的合成事件属性！当事件行为触发，根据原生事件都会传播到`#root`容器上，`React`内部给`#root`容器做了事件绑定**捕获和冒泡**

当React内部绑定的方法执行的时候，会依据`ev.path`中分析的路径依次把对应阶段的`onXxx/onXxxCapture`等事件合成属性触发！

合成事件是利用事件委托（事件传播机制）完成的！！

- 探索合成事件中的事件阻止属性

```jsx
//./src/view/Demo.jsx
import React from 'react'

class Demo extends React.Component {

	render(){
		return(
			<div className="outer" onClick={()=>{
                        console.log('outer 冒泡')
                    }} onClickCapture={()=>{
                        console.log('outer 捕获')
                    }}>
                <div className='inner' onClick={(ev)=>{
                        //ev:合成事件对象
                        console.log('inner 冒泡')
                        //ev.stopPropagation()//合成事件中的阻止事件传播：阻止原生的事件传播，阻止合成事件中的事件传播
                        //ev.nativeEvent.stopPropagation()//原生事件对象中的“阻止事件传播”，只能阻止原生事件
                        //ev.nativeEvent.stopImmediatePropagation()//原生事件对象中的“阻止事件传播”，只不过可以阻止#root上的其他绑定方法执行
                    }} onClickCapture={()=>{
                        console.log('inner 捕获')
                    }}></div>
			</div>
		)
	}
    compontentDidMount(){
        document.addEventListener('click', ()=>{
            console.log('document 捕获')
        }，true);
        document.addEventListener('click', ()=>{
            console.log('document 冒泡')
        }，false);
        document.body.addEventListener('click', ()=>{
            console.log('body 捕获')
        }，true);
        document.body.addEventListener('click', ()=>{
            console.log('body 冒泡')
        }，false);
        
        let root = document.queryselector('#root');
        root.addEventListener('click', ()=>{
            console.log('root 捕获')
        }，true);
        root.addEventListener('click', ()=>{
            console.log('root 捕获')
        }，false);
        
        //这里只要使用到EV就是原生对象事件，使用的都是js内部的事件方法
    }
}
export default Demo

```

> [!CAUTION]
>
> 在16版本，合成事件的处理机制，不再是把事件委托给`#root`对象，而是委托给`document`对象，并且只在冒泡阶段的进行委托，在委托的方法中，把`onXxx/onXxxCapture`合成事件属性进行执行，这里冒泡和捕获的所有方法均写在了冒泡阶段的函数中，而18中，分别在冒泡和捕获阶段均设置相应的处理函数
>
> 在16版本中，关于合成事件对象的处理，`React`内部是基于事件对象池，做了一个缓存机制，`React17`以后，是去掉了这套事件对象池和缓存机制，当每一次事件触发的时候，如果传播到了委托的元素上（`document/#root`），在委托的方法中，我们首先会对内置事件对象做统一处理，生成合成事件对象，而在16版本中，为了防止每一次都是重新创建出的新的事件对象，它设置了一个事件对象池，本次事件触发，获取到事件操作的相关信息，我们从事件对象池中，获取存储的合成事件对象，把信息赋值给相关的成员信息，等待本次操作结束，把合成事件对象中的成员信息都清空掉，再放入到事件对象池中
>
> 在16版本，`ev.persist()`方法可以将合成事件对象中的信息保留下来

- 移动端细节问题

移动端的`click`是单击事件，`pc`端的`click`是点击事件

连续点击两下：

​	PC端会触发：两次click事件，一次dblclick事件

​	移动端会触发：不会触发click，指挥触发dblclick事件，

​	点击事件的原理：第一次点击后，监听`300ms`，看是否有第二次点击操作，如果没有就是单击，如果有就是双击

单手指事件模型：`touch`

`touchstart`、`touchmove`、`touchend`

```jsx
//./src/view/Demo.jsx
import React from 'react'

class Demo extends React.Component {
	// 手指按下：记录下手指的起始坐标
    touchstart = (ev) =>{
        console.log(ev)
        let finger = ev.changedTouches[0]//记录了操作手指的相关信息
        this.touch = {
            startX:finger.pageX
            startY:finger.pageY
            isMove:false
        }
    }
    //手指移动,记录手指偏移值，和误差值做对比，分析出是否发生移动
    touchmove = (ev) =>{
		let finger = ev.changedTouches[0]，//记录了操作手指的相关信息
        	{ startX, startY } = this.touch
        let changeX = finger.pageX - startX,
            changeY = finger.pageY - startY;
        if(Math.abs(change)>10 || Math.abs(change)>10){
            this.touch.isMove = true
        }
    }
    //手指离开,根据isMove判断是否是点击
    touchend = () =>{
        let { isMove } = this.touch;
        if (isMove) return
        console.log('点击了按钮')
    }
    
	render(){
		return(
			<div>
				<button onTouchStart={this.touchstart} 
                    onTouchMove={ this.touchmove } 
                    onTouchEnd={this.touchend}>提交</button>
			</div>
		)
	}

}
export default Demo

```

```jsx
//./src/view/Demo.jsx
import React from 'react'

import FastClick from 'fastclick'
FastClick.attach(document.body) //使用FastClick解决移动端click事件的300ms延迟问题

class Demo extends React.Component {
	
    handle = () => {
        console.log('点击了按钮')
    }
    
	render(){
		return(
			<div>
				<button onClick = {this.handle}>提交</button>
			</div>
		)
	}

}
export default Demo
```

- 循环节点绑定点击事件

```jsx
//./src/view/Demo.jsx
import React from 'react'

class Demo extends React.Component {
	state = {
        arr: [{
            id:1,
            title:'新闻'
        },{
            id:2,
            title:'体育'
        },{
            id:3,
            title:'电影'
        }]
    }
    handle = (item) =>{
        console.log('我点击的是' + item.title)
    }
    
	render(){
        let { arr } = this.state
		return(
			<div>
				{arr.map(item => {
                    let {id,title} =item;
                    return(
                    	<span key={id} style={{
                                padding:'5px 15px',
                                marginRight:10,
                                border:'1px solid #DDD',
                                cursor:'pointer'
                            }} onClick={
                                this.handle.bind(this,item)
                            }>{title}
                        </span>
                    )
                })}
			</div>
		)
	}
}
export default Demo
```

按照正常，我们再给循环节点创建点击事件，是按照事件委托的方式进行触发点击事件，但是在React中，我们不需要考虑这些，我们循环给元素绑定的合成事件，本身就是基于事件委托处理的，所以无需我们再单独的设置事件委托的处理机制

在`vue`中，没有委托机制，最好是按照`js`原生的方式，在循环体容器上设置事件委托机制

### 16、练习：`TASK OA`系统

技术栈：`React`、`Antd`, 服务端`pm2`进行服务器状态管理， 发送请求用`Axios`

`UI` 组件库，是把项目中常用的功能封装成一个个的组件【结构、样式、功能】，后期项目开发中，我们基本是必用`UI`组件库

- `React` `UI`组件库
  - PC端：`Antd`、`AntdPro`
  - 移动端：`AntdMobile`

- Vue UI组件库
  - PC端：`element-ui`、`antd for vue`、`iview`
  - 移动端：`vant`、`cube`

`antd`组件库 自带按需导入，我们安装整个`antd`，后期在项目中用到哪些组件，最后打包的时候，只打包用的

修改UI组件库的样式：

> 1、找到渲染后的内容，我们去观察它的样式类和相关样式
>
> 2、按照样式类去修改样式，只要保证我们自己写的样式权重高即可，必要时可以使用`!important`方式在`css`中提高的最高

```jsx
// ./src/index.jsx
import React from 'react'
import ReactDOM from 'react-dom/client'
import Task from './views/Task'
//使用ANTD组件库
import { ConfigProvider } from 'antd' //配置库
import zhCN from 'antd/locale/zh_CN' //中文配置
import './src/index.less'

const root = ReactDOM.createRoot(document.getElementById('app'))
root.render(
	<ConfigProvider locale={ zhCN }>
        <Task />
    </ConfigProvider>
)
```

```less
// ./src/index.less 

@import './assets/reset.min.css';

.ant-btn {
    border-radius: 0;
    $.ant-btn-sm {
        border-radius: 0;
    }
}

.ant-picker,
.ant-input{
    border-radius: 0;
}

textarea{
    resize: none;
}
```



```jsx
// ./src/views/Task.jsx

import React from 'react'
import './Task.less'
import { Button, Form, Input, Modal, Table, Tag, Popconfirm, DatePicker, message } from 'antd'//你想用哪个组件就导入哪个
import { getTaskList, addTask, removeTask, completeTask} from '@api/index'

//对日期处理方法
const zero = function zero(text) {
    text = String(text)
    return text.length<2? '0'+text : text;
}
const formatTime = function formatTime(time){
    let arr = time.match(/\d+/g)
    let [,month,day,hours = '00',minutes = '00'] = arr
    return `${zero(month)}-${zero(day)} ${zero(hours)}:${zero(minutes)}`
}

class Task extend React.compontent{
    //表格列的数据
    columns = [{
        title:'编号',
        dataIndex:'id',
        align: 'center',
        width: '8%',
    },{
        title:'任务描述', 
        dataIndex:'task',
        ellipsis: true,
        width: '50%',
    },{
        title:'状态',
        dataIndex:'state',
        align: 'center',
        width: '10%',
        render:text => {
        	return +text===1?'未完成' : '已完成'	
        }
    },{
        title:'完成时间',
        dataIndex:'time',
        align: 'center',
        width: '15%',
        render:(_, record) => {
            let {state, time, complete} = record
            if(+state === 2 ) time = complete
            return formatTime(time)
        }   
    },{
        title:'操作',
        render:(_,record) => {
            let { id, state } = record
            return(
            	<>
                	<Popconfirm title="您确定要删除此任务么？" 
                        onConfirm={this.handleRemove.bind(this,id)}>
                    	<Button type="link">删除</Button>
                	</Popconfirm>
                	{+state !==2? <Popconfirm title="您确定把此任务设置成完成吗？" 
                                    	onConfirm={this.handleUpdate.bind(this,id)}>
                    	<Button type="link">完成</Button>
                	</Popconfirm>:null}
            	</>
            )
        }
    }]
    //初始组件的状态
    state={
        tableDate:[{
            id:1,
            task:"今天天气很不错，今夜阳光明媚"
            state:1,
            time:'2022-11-29 18:00:00'
            complete:"2022-11-29 18:00:00"
        }],
        tableLoading:false,
        modalVisible:false,
        confirmLoading:false,
        selectedIndex:0,
    };
	//对话框和表单处理的方法
	closeModal =() =>{ //关闭对话框&清除表单内容
        this.setState({
            modalVisible:false,
            confirmLoading:false
        })
        this.formIns.resetFields()
    }
    submit = async() => {  //新增任务
        try{
            //表单校验
            await this.formIns.validateFields();
        	let { task, time} = this.formIns.getFieldsValue()
            time=time.format('YYYY-MM-DD HH:mm:ss')
            //向服务器发送请求
            this.setState({ confirmLoading: true })
            let { code } = await addTask(task, time)
            if (+code !== 0 ){
                message.error('很遗憾，当前操作失败，请稍后再试')
            }else{
                this.closeModal()
                this.queryData()
                message.success('恭喜您，当前操作成功')
            }
        }catch(_){
            
        }
        this.setState({ confirmLoading: false })
    }
    //从服务器获取指定状态的任务
    queryData = async() => {
        let { selectedIndex } = this.state;
        try{
            this.setState({ tableLoading:true})
        	let { code, list } = await getTaskList(selectedIndex)
            if(+code!==0) list=[];
            this.setState({
                tableData: list
            })
        }catch(_){}
        this.setState({ tableLoading:false})
        
    }
    //选中切换状态
    changeIndex= () => {
        if(this.state.selectedIndex === index) return
        this.setState({
            selectedIndex: index
        },()=>{
 	       this.queryData()         
        })
        //另一种方式
        //flushSync(()=>{
        //    this.setState({
        //        selectedIndex: index
        //    })
        //})
        //this.queryData() 
    }
    //删除
    handleRemove = async(id) => {
        try{
            let { code } = await removeTask(id) 
            if(+code !== 0){
                message.error('很遗憾，操作失败，请稍后再试')
            }else{
                this.queryData()
                message.success('恭喜您，操作成功！')
            }
        }catch(_){
            
        }
    }
    //完成
    handleUpdate = async(id) => {
        try{
            let { code } = await completeTask(id) 
            if(+code !== 0){
                message.error('很遗憾，操作失败，请稍后再试')
            }else{
                this.queryData()
                message.success('恭喜您，操作成功！')
            }
        }catch(_){
            
        }
    }
    //周期函数
    compontentDidMount(){
        this.queryData
    }
    render(){
        console.log('RENDER')
        let { tableData, tableLoading, modalVisible, confirmLoading, selectedIndex } = this.state;
        <div className="task-box">
            <div className="header">
                <h2 className='title'>TASK OA 系统任务管理系统</h2>
                <Button type="primary" onClick={()=>{
                        this.setState({
                            modalVisible = true
                        })
                    }}>新增任务</Button>
            </div>
            <div className='tag-box'>
                {['全部','未完成','已完成'].map((item, index) => {
                    return (
                        <Tag key={ index } 
                             color={ selectedIndex === index? '#1677ff' : '' } 
                             onClick={this.changeIndex.bind(null,index)}
                               >{ item }</Tag>
                           )
                })}             
            </div>
            <Table dataSource={ tableData } columns={ this.columns } loading={tableLoading} pagination={ false } rowKey='id' />
            <Modal title="新增任务窗口" open={ modalVisible } confirmLoading={ confirmLoading } keyboard= { false } maskClosable={ false } okText='确认提交' onCancel={ this.closeModal } onOk={ this.submin }>
                <Form layout="vertical" initialValues={{
                        task: '',
                        time:''
                    }} >
                	<Form.Item ref={x => this.formInx = x} label="任务描述" name="task" validateTigger="onBlur" rules={[
                            {require: true,message:'任务描述必填'},
                            {min:6,message:'输入的内容至少6位及以上'}
                        ]}>
                        <Input.TextArea rows={ 4 }></Input.TextArea>
                    </Form.Item>
                    <Form.Item label="预期完成时间" name="time" validateTigger="onBlur" rule={[
                            {require: true,message:'预期完成事件是必填项'}
                        ]}>
                        <DatePicker showTime />
                    </Form.Item>
                </Form>
            </Modal>
        </div>
    }
}
export default Task
```

```less
// ./src/views/Task.less
.task-box {
    box-sizing: border-box;
    margin: 0 auto;
    width: 800px;
    
    .header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding-bottom: 10px;
        border-bottom: 1px solid #DDD;
        
        .title {
            font-size: 20px;
            line-height: 50px;
        }
    }
    
    .tag-box {
        margin: 15px 0;
        
        .ant-tag {
            margin-inline-end: 20px;
            padding-inline: 10px;
            border-radius: 0;
            font-size: 14px;
            line-height: 30px;
            cursor: pointer;
            
        }
    }
    .ant-table-cell{
        .ant-btn-link{
            padding: 4px;
        }
    }
}

```

```js
// ./src/api/http.js
import axios from 'axios';
import qs from 'qs';
import { message } from 'antd';
import _ from '@/assets/utils';

const http = axios.create({
    baseUrl: '/api',
    timeout:60000
});
http.defaults.transformRequest = data => {
    if (_.isPlainObject(data)) data= qs.stringify(data);
    return data;
}
http:interceptors.response.use(response => {
    return response.data;
},reason => {
    message.error('当前网络繁忙，请您稍后再试')
    return Promise.reject(reason)
});
export default http
```

```js
// ./src/api/index.js
import http from './http'

//获得指定状态的任务信息
export const getTaskList =(state=0) => {
    return http.get('/getTaskList', {
        params: {
            state
        }
    })
}

//新增任务
export const addTask = (task,time) => {
    return http.post('/addTask',{
        params:{
            task,
            time
        }
    })
}

//删除任务
export const removeTask = (id) => {
    return http.get('/removeTask',{
        params:{
            id
        }
    })
}

//完成任务
export const completeTask = (id) => {
    return http.get('/completeTask',{
        params:{
            id
        }
    })
}
```



## 三、React高级

### 1、`React Hooks`组件开发

`React Hooks`组件 就是基于`React`中新提供的`Hook`函数，可以让函数组件动态化

- 基础`Hook`

  - `useState` 使用状态管理
  - `useEffect` 使用周期函数
  - `useContext` 使用上下文

- 额外的`Hook`

  - `useReducer` `useState`的替代方案，借鉴`Redux`处理思想，管理更为复杂的状态和逻辑
  - `useCallback` 构建缓存优化方案
  - `useMemo` 构建缓存优化方案
  - `useRef` 使用`ref`获取`DOM`
  - `useImperativeHandle` 配合`forwardRef`（`ref`转发）一起使用
  - `useLayoutEffect` 与`useEffect`相同，但会在所有的`DOM`变更之后同步调用`effect`

  ...

- 自定义`Hook`

  ...

### 2、`useState`函数

`useState`：`React Hook` 函数之一，目的是在函数组件中使用状态，并且后期基于状态的修改，可以让组件更新

```jsx
let [num, setNum] = useState(initialValue) //返回是一个数组
//执行useState，传递的initialValue是初始的状态值
//执行这个方法，返回结果是一个数组:[状态值，修改状态的方法]
//	这里num就是状态值，setNum就是修改状态的方法
//执行setNum(value)
//	修改状态值为value
//  通知视图更新
//函数组件(hook组件)不是类组件，所以没有实例的概念，调用组件不再是创建类的实例，而是把函数执行，产生一个私有上下文而已，在函数组件中不涉及this的处理
```



```jsx
// ./src/index.jsx

import React from 'react';
import ReactDOM from 'react-dom/client';
import Demo from './views/Demo';
//使用ANTD组件库
import {configProvider} from 'antd';
import zhCN from 'antd/locale/zh_CN';
import './index.less'

const root = ReactDOM.createRoot(document.getElementById('root'))
root.render(
	<ConfigProvider locale={zhCN}>
        <Demo />
    </ConfigProvider>
);
```

```less
@import './assets/reset.min.css';

//全局样式
.ant-btn {
	border-radius: 0;
    
    $.ant-btn-sm {
        border-radius: 0
    }
}

.ant-picker,
.ant-input{
    border-radius: 0;
}

textarea{
    resize: none;
}

```



```jsx
// ./src/views/Demo.jsx

import React, { useState } from 'react'
import { Button } from 'antd'
import './Demo.less'

const Demo = function Demo(){
    let [num, setNum] = useState(0)
    
    const handle = () => {
        setNum(num+10)
    }
    
    return (
    	<div className= "demo">
            <span className="num">{ num }</span>
            <Button type="primary" size="small" onClick={ handle }>新增</Button>
        </div>
    )
}
//如果使用静态组件
//const Demo = function Demo(){
//    let n = 0
//    return (
//    	<div className= "demo">
//            <span className="num">{n}</span>
//            <Button type="primary" size="small" onClick={() => {
//                    n += 10;
//                }}>新增</Button>
//        </div>
//    )
//}

//如果使用类组件的写法
//class Demo extends React.Component{
//    state = {
//        n:0
//    }
//    handle = () => {
//        let {n} =this.state
//        this.setState({
//            n:n  +10
//        })
//    }
//    render(){
//        let {n} =this.state
//        return(
//           <div className= "demo">
//                <span className="num">{ n }</span>
//                <Button type="primary" size="small" onClick={handle}>新增</Button>
//            </div>
//        )
//    }
//}
export default Demo
```

```less
.demo {
	box-sizing:border-box;
	margin:10px auto;
    padding: 10px;
    width: 200px;
    border: 1px solid #CCC;
    
    .num {
        display: block;
        font-size: 20px;
        line-height: 40px;
    }
}
```

机制研究：

函数组件的每一次渲染或者更新都是把函数重新执行，产生一个全新的私有上下文（作用域）

> 内部的代码都必须重新执行
>
> > 第一次渲染组件把组件执行，传递属性
> >
> > 产生一个私有上下文，`EC(DEMO)`
> >
> > ```js
> > //私有变量：
> > num = 0
> > setNum = 修改状态的函数
> > handle = 函数
> > //开始编译JSX视图，创建virtualDOM，最后渲染真实DOM
> > 
> > //点击新增按钮，执行handle方法
> > //上级上下文EC(DEMO)
> > //私有作用域，无变量，仅有一个setNum函数，但本作用域没有这个函数方法，就往上个作用查找此函数
> > setNum(0+10) -> setNum(10)
> > 
> > //组件更新，再次执行组件函数
> > //这一次执行useState并不是使用初始值0.而是返回新修改的状态值。let [num, setNum] = useState(10)
> > //此时setNum获取的是修改状态的新函数，即每次点击，都会生成一个新的函数地址，与之前的函数并不是同一个。
> > ```
>
> 涉及的函数需要重新的构建，这些函数的作用域（作用上下文），是第一次执行`DEMO`的产生的闭包。
>
> 每一次执行`DEMO`函数，也会把`useState`重新执行，但是执行`useState`，只有第一次，设置的初始值会生效，其余以后再执行，获取的状态都是最新的状态值，而不是初始值，返回的修改状态的方法，每一次都是返回一个新的。

```jsx
// ./src/views/Demo.jsx

import React, { useState } from 'react'
import { Button } from 'antd'
import './Demo.less'

const Demo = function Demo(){
    let [num, setNum] = useState(0)
    
    const handle = () => {
        setNum(num+10)
    }
    
    return (
    	<div className= "demo">
            <span className="num">{ num }</span>
            <Button type="primary" size="small" onClick={ handle }>新增</Button>
        </div>
    )
}
export default Demo
```

```js
//useState的原理
var _state;
function useState(initialValue) {
	if(typeof _state === 'undefined') _state = initialValue;
	var setState = function setSTate(value){
		_state = value
		//通知视图更新
	}
	return [_state, setState]
}
```



- 多个状态数据的更新处理

```jsx
// ./src/index.jsx

import React ,{ useState } from 'react';
import ReactDOM from 'react-dom/client';
import Vote from './views/Vote';
//使用ANTD组件库
import {configProvider} from 'antd';
import zhCN from 'antd/locale/zh_CN';
import './index.less'

const root = ReactDOM.createRoot(document.getElementById('root'))
root.render(
	<ConfigProvider locale={zhCN}>
        <Vote title="React学好则需要JS功底！"/>
    </ConfigProvider>
);
```

```jsx
//./src/views/Vote.jsx

const Vote =function Vote(props){
    //执行一次useState，把需要的状态信息都放在对象中
    //  执行setState方法的时候，传递的是啥值，就把状态整体改为啥值
    //  setState({
    //		supNum:state.supNum + 1	
	//  })
    //  => 把状态值修改为{supNum:11}, oppNum成员就丢失了
    //  => 并不会像类组件中的this.setState一样，不支持部分状态的更新
    //  应该改为以下处理方案
    //  setState({
    //      ...state,  //在修改值之前，先把原有的所有状态，都展开赋值给新对象，再去修改要改动的那一项值即可
    //      supNum:state.supNum+1
    //  })
    //官方建议，需要多个状态值，就把useState执行多次即可
    //	let [supNum, setSupNum] = useState(10)
    //	let [oppNum, setOppNum] = useState(5)
    //	再根据每个独立的状态进行操作
    let [state, setState] = useState({
        supNum: 10,
        oppNum: 5
    })

    const handle = (type)=>{
       if(type==='sup'){
           setState({
               ...state,
               supNum:state.supNum+1
           })
       }else if(type === 'opp'){
           setState({
               ...state,
               oppNum:state.oppNum+1
           })
       }
    }
    return(
    	<div className="vote-box">
            <div className="header">
                <h2 className="title">{ props.title }</h2>
                <span>{ supNum + oppNum }</span>
            </div>
            <div className="main">
                <p>支持人数：{ state.supNum }人</p>
                <p>反对人数：{ state.oppNum }人</p>
            </div>
            <div className="footer">
                <button onClick={handle.bind(null,'sup')}>支持</button>
                <button onClick={handle.bind(null,'opp')}>不支持</button>
            </div>
        </div>
    )
}

export default Vote
```

- `setState`的异步处理机制

```jsx
// ./src/views/Demo.jsx

import React, { useState } from 'react'
import { Button } from 'antd'
import './Demo.less'
import { flushSync } from 'react-dom'

const Demo = function Demo(){
    console.log('Render渲染') //执行一次，就渲染一次xyz，说明该setX,setY,setZ都是异步操作
    let [x, setX] = useState(10)
    let [y, setY] = useState(20)  
    let [z, setZ] = useState(30)
    
    const handle = () => {
        flushSync(() => {
            setX(x + 1);
            setY(y + 1);
        });//遇到flushSync立即刷新更新队列，进行渲染。
        setZ(z + 1);
    }
    
    return (
    	<div className= "demo">
            <span className="num">x:{x}</span>
            <span className="num">y:{y}</span>
            <span className="num">z:{z}</span>
            <Button type="primary" size="small" onClick={ handle }>新增</Button>
        </div>
    )
}
export default Demo
```

在`React18`中，我们基于`useState`创建出来的“修改状态的方法”，它们的执行也是异步的，原理：等同于类组件中的`this.setState`，基于异步操作&更新队列，实现状态的批处理，在任何地方修改状态，都是采用异步编程的

在`React16`中，也和`this.setState`一样，放在合成事件/周期函数中，是异步的操作，放在其他异步操作（定时器，手动事件绑定）中，是同步的

- 在循环体里的机制

```jsx
// ./src/views/Demo.jsx

import React, { useState } from 'react'
import { Button } from 'antd'
import './Demo.less'
import { flushSync } from 'react-dom'

const Demo = function Demo(){
    let [x, setX] = useState(10)

    
    const handle = () => {
//		for(let i = 0 ; i<10; i++){
//		flushSync(()=>{
//        	setX(x+1)
//    	})//1、利用flushSync更新视口，无论循环多少次，其结果都是只渲染一次，结果是10+1，原因是handle的作用域中只能调用x为10
//            setX(x+1)
//        }//2、即便不用flushSync，采用普通循环，这里无论循环多少次，其结果都是只渲染一次，结果是10+1，原理同上
        
        //setX(10)//3、这里如果更新的值和之前的状态值做比较，基于Object.is作比较，如果两次的值一样，则不会修改状态，可以理解为PureCompontentUpdate中做了浅比较和优化
        
        setX(prev => {//4、采用x=>x+1的函数可以在一次视窗更新内多次累加
            //prev:储存的是上一次的状态值
            console.log(prev)
            return prev+1 //返回的信息是我们要修改的值
        })
    }
    
    return (
    	<div className= "demo">
            <span className="num">x:{x}</span>
            <Button type="primary" size="small" onClick={ handle }>新增</Button>
        </div>
    )
}
export default Demo
```

### 3、`useEffect`函数

在函数组件中，使用生命周期函数

```jsx
// ./src/views/Demo.jsx

import React, { useState } from 'react'
import { Button } from 'antd'
import './Demo.less'

const Demo = function Demo(){
    
    let [num, setNum] = useState(0)
    let [x, setX] = useState(100)
    
    useEffect(()=>{
        //获取最新的状态值
        console.log('1'，num)
    })
    //1、useEffect(()=>{})第一次渲染完毕，执行()=>{},等价于 compontDidMount
    //在组件每一次更新完毕后，也会执行()=>{}，等价于componentDidUpdate
    
    useEffect(()=>{
        console.log('2'，num)
    },[])
    //2、useEffect(()=>{},[])只有在第一次渲染完毕后，才会执行()=>{}，每一次组件视图更新完毕后，()=>{}不再执行
    //类似于 cpmponentDidMount
    
    useEffect(()=>{
        console.log('3'，num)
    },[num])
    //3、useEffect(()=>{},[依赖的状态])，第一次渲染完毕后会执行()=>{}，当依赖的状态值（或者多个依赖状态的一个）发生改变，也会触发()=>{}，当时依赖的状态如果没有变化，在组件更新的时候，()=>{}不会执行
    useEffect(()=>{
        return() =>{
            //获取的未改变前的值
            console.log('4'，num)
        }
    },[num])
    //4、返回的小函数，会在组件释放的时候执行，
    //如果组件更新，会把未改变时候的小函数执行，
    const handle = () => {
		setNum(num + 1)
    }
    
    return (
    	<div className= "demo">
            <span className="num">x:{x}</span>
            <Button type="primary" size="small" onClick={ handle }>新增</Button>
        </div>
    )
}
export default Demo
```

第一次渲染的时候

```js
num = 0      setNum函数
x=100          setX函数                            effect链表(通过MountEffect方法把callback依赖项加入链表中)

useEffect(()=>{                        |		1、callback   没设置依赖
    console.log('1',num)               |
})									   |
									   |
useEffect(()=>{                        |        2、callback   无依赖
    console.log('2'，num)              |
},[])                                  |
                                       |
useEffect(()=>{                        |        3、callback   依赖num 
    console.log('3'，num)              |
},[num])                               |
                                       |
useEffect(()=>{                        |        callback    没设置依赖，没有任何输出
    return() =>{                       |		4、返回一个小函数
        console.log('4'，num)          |
    }                                  | 
},[num])                               |
...    
视图渲染完毕     基于updateEffect方法，通知effect链表中的callback按照要求执行

输出
第一次渲染
1 0
2 0
3 0
点击按钮更新组件
1 1
3 1
4 0  上一个组件销毁，所以执行了4，4按照上一个组件的作用域进行执行结果
```

```jsx
// ./src/views/Demo.jsx

import React, { useState,useEffect } from 'react'
import { Button } from 'antd'
import './Demo.less'

const queryData=()=>{
    return new Promise(resolve => {
        setTimeout(()=>{
            resolve([10,20,30])
        },1000)
    })
}

const Demo = function Demo(){
    
    let [num, setNum] = useState(0)
    
    //以下条件判断会报错
    //useEffect 必须在函数的最外层上下文中调用，不能把其嵌入到条件判断，循环等操作语句中
    //if(num>5){
    //    useEffect(()=>{
    //        console.log('ok')  //这样书写会报错
    //    })
    //}
    //useEffect应该包括条件函数
    useEffect(()=>{
        if(num>5){
            console.log('ok')
        }
    })
    
    //第一次渲染完毕后，从服务器异步获取数据
    //useEffect如果设置返回值，则返回值必须是一个函数，（代表组件销毁时触发）；下面案例中，callback经过async修饰，返回的是一个promise实例，不符合要求
    //useEffect(()=>{
    //    let date = await queryData()
    //    console.log('成功',data)
    //},[])
    useEffect(()=>{
        queryData().then(data => {
            console.log('成功', data)
        })
        //或者
        //const next =async () => {
        //    
        //};
        //next()
    },[])
    const handle = () => {
		setNum(num + 1)
    }
    
    return (
    	<div className= "demo">
            <span className="num">{num}</span>
            <Button type="primary" size="small" onClick={ handle }>新增</Button>
        </div>
    )
}
export default Demo
```

- `useLayoutEffect()`

```jsx
// ./src/views/Demo.jsx

import React, { useState, useEffect, useLayoutEffect } from 'react'
import { Button } from 'antd'
import './Demo.less'

const Demo = function Demo(){
    
    console.log('Render')
    
    let [num, setNum] = useState(0)
    
    //useLayoutEffect(()=>{   //会在virtualDOM的时候将uselayoutEffect加入链表，而useEffect在创建真实DOM后加入链表
    //if (num === 0){
    //    setNum(10)
    //}
    //},[num])
    //uselayoutEffect会阻塞浏览器渲染真实DOM，优先执行Effect链表中的callback
    //useEffect不会阻塞浏览器渲染，在渲染真实DOM的同时，去执行Effect链表中的callback
    	//useLayoutEffect设置的callback要优于useEffect去执行！
    	//在两者设置的callback中，依然可以获取DOM元素，（真实DOM对象已经创建，区别只是浏览器是否渲染）
    	//如果在callback函数中又修改状态值
    		//useEffect浏览器肯定是把第一次的真实DOM已经绘制了，再去渲染第二次的真实DOM
    		//useLayoutEffect浏览器是把两次真实DOM渲染了，合并在一起渲染的
    useLayoutEffect(()=>{
        console.log('useLayoutEffect')
        console.log(document.queryselector('num'))
    })
    useEffect(()=>{
        console.log('useEffect')
        console.log(document.queryselector('num'))
    })

    const styleObj = {
        color: num === 0? 'red' : 'green'
    }
    const handele = () => {
        setNum(0)
    }
    return (
    	<div className= "demo">
            <span className="num" style={styleObj}>{num}</span>
            <Button type="primary" size="small" onClick={ handle }>新增</Button>
        </div>
    )
}
export default Demo
```

如果链表中的`callback`执行又修改了状态值，对于`useEffect`来讲，第一次真实`DOM`已经渲染，组建更新会重新渲染真实的`DOM`，所以频繁切换的时候，会出现闪烁的效果，对于`useLayoutEffect`来讲，第一次真实`DOM`还未渲染，遇到`callback`中修改了状态，试图立即更新，创建出新的`virtualDOM`，然后和上一次的`virtualDOM`合并在一起渲染出真实`DOM`，也就是此类需求下，真实`DOM`只渲染了一次，不会出现内容/样式的闪烁。

### 4、`useRef`函数

类组件中，我们基于`ref`可以做的事情：

> 赋值给一个标签：获取`DOM`元素 
>
> > `ref="box"`
> >
> > `this.refs.box`
>
> 赋值给一个类子组件：获取子组件实例，可以基于实例调用子组件的属性和方法
>
> > `ref=(x=>this.box=x)`
> >
> > `this.box`获取
>
> 赋值给一个函数子组件：报错[需要配合`React.forwardRef`实现`ref`转发，获取子组件中的某一个`DOM`元素
>
> > `this.box = React.createRef()`
> >
> > `<h2 ref=(this.box)></h2>`
> >
> > `this.box.current`获取DOM元素

- 方法一：基于函数方式

```jsx
// ./src/views/Demo.jsx

import React, { useState, useEffect, useRef } from 'react'
import { Button } from 'antd'
import './Demo.less'

const Demo = function Demo(){
    let [num, setNum] = useState(0)
	
    //基于ref={函数}的方式，可以把创建的DOM元素或者（子组件的实例）赋值给box变量，不推荐使用
    let box;
    useEffect(()=>{
       console.log(box) 
    },[])
    const styleObj = {
        color: num === 0? 'red' : 'green'
    }
    const handele = () => {
        setNum(0)
    }
    return (
    	<div className= "demo">
            <span className="num" style={styleObj} ref={x => box=x }>{num}</span>
            <Button type="primary" size="small" onClick={ handle }>新增</Button>
        </div>
    )
}
export default Demo
```

- 方法二：基于`React.createRef()`

```jsx
// ./src/views/Demo.jsx

import React, { useState, useEffect, useRef } from 'react'
import { Button } from 'antd'
import './Demo.less'

const Demo = function Demo(){
    let [num, setNum] = useState(0)
    
    //也可以基于React.createRef创建ref对象来获取想要的内容
    let box =React.createRef()

    useEffect(()=>{
       console.log(box.current) 
    },[])
    const styleObj = {
        color: num === 0? 'red' : 'green'
    }
    const handele = () => {
        setNum(0)
    }
    return (
    	<div className= "demo">
            <span className="num" style={styleObj} ref={x => box}>{num}</span>
            <Button type="primary" size="small" onClick={ handle }>新增</Button>
        </div>
    )
}
export default Demo
```

- 方法三：使用`hooks`函数

```jsx
// ./src/views/Demo.jsx

import React, { useState, useEffect, useRef } from 'react'
import { Button } from 'antd'
import './Demo.less'

const Demo = function Demo(){
    let [num, setNum] = useState(0)
    
    //函数组件中，还可以基于 useRef Hook函数，创建一个ref对象
    //	React.creatRef 也是创建ref对象，即可在类组件中使用，也可以在函数组件中使用
    //	useRef 只能在函数组件中用
    let box = useRef(null)

    useEffect(()=>{
       console.log(box.current) 
    },[])
    const styleObj = {
        color: num === 0? 'red' : 'green'
    }
    const handele = () => {
        setNum(0)
    }
    return (
    	<div className= "demo">
            <span className="num" style={styleObj} ref={box}>{num}</span>
            <Button type="primary" size="small" onClick={ handle }>新增</Button>
        </div>
    )
}
export default Demo
```

- `createRef` Vs `useRef`

```jsx
// ./src/views/Demo.jsx

let prev1,
    prev2
import React, { useState, useEffect, useRef } from 'react'
import { Button } from 'antd'
import './Demo.less'

const Demo = function Demo(){
    let [num, setNum] = useState(0)
    let box1 = useRef(null),
        box2 = React.createRef()
	if(!prev1) {
        //第一次Demo执行，把第一次创建的Ref对象赋值给变量
        prev1 = box1;
        prev2 = box2
    }else{
        //第二次Demo执行，我们验证一下，新创建的Ref对象，和之前第一次创建的Ref对象，是否一致
        console.log(prev1 === box1) //true useRef在每一次组件更新的时候（函数重新执行），再次执行useRef方法的时候，不会创建新的Ref对象了，获得的还是第一次创建的那个ref对象
        console.log(prev2 === box2) //false createRef在每一次组件更新的时候，都会创建一个全新的Ref对象出来，比较浪费性能
    }
    useEffect(()=>{
       console.log(box1.current) 
       console.log(box2.current)
    },[])
    const styleObj = {
        color: num === 0? 'red' : 'green'
    }
    const handele = () => {
        setNum(0)
    }
    return (
    	<div className= "demo">
            <span className="num" style={styleObj} ref={box1}>{num}</span>
            <span className="num" style={styleObj} ref={box2}>{num}</span>
            <Button type="primary" size="small" onClick={ handle }>新增</Button>
        </div>
    )
}
export default Demo
```

> [!NOTE]
>
> 总结：在类组件中，创建Ref对象，我们基于`React.createRef`处理；但是在函数组件中，为了保护性能，我们应该使用专属的`useRef`处理

- `useImperativeHandle`函数

```jsx
// ./src/views/Demo.jsx
import React, { useState, useEffect, useRef } from 'react'
import { Button } from 'antd'
import './Demo.less'

//1、如果是类组件，直接获取的是ref实例
class Child extends React,Component {
    state = { x:1000 }
    render(){
        return (
            <div className='child-box'>{ this.state.x }</div>
        )
    }
}

//2、如果是函数组件，需要通过forwardRef进行转发ref，直接调用ref，会报错，必须用forwardRef
const Child = React.forwardRef(function Child(props,ref){
    console.log(ref)  //在Demo中，调用Child的时候，传递的ref对象[x]
    return (
    	<div className='child-box'>
            <span ref={ref}></span>
        </div>
    )
})

//3、如何实现子组件内部，基于forwardRef实现ref转发的同时，获取子组件内部的状态或者方法呢？
//一个新的hook函数
const Child = React.forwardRef(function Child(props,ref){
    let [text, setText] = useState('你好世界')]
    const submit = () => {
        
    }
    
    useImperativeHandle(ref, ()=>{
        //在这里返回的内容，都可以被父组件的Ref对象获取到
        return {
            text,
            submit
        }
    })
    return (
    	<div className='child-box'>
            <span ref={ref}></span>
        </div>
    )
})

const Demo = function Demo(){
	let x = useRef(null)

    useEffect(()=>{
        console.log(x.current)//这样就能拿到3中的text和submit属性了
    },[])

    return (
    	<div className= "demo">
			<child ref={x}/>
        </div>
    )
}
export default Demo
```

> [!NOTE]
>
> 基于forwardRef实现Ref转发，目的是获取子组件内部的某个元素
>
> 基于ref获取子组件的实例，这样基于实例可以调用子组件内部挂载到实例上的东西

### 5、`TASK OA`系统改写

```jsx
// ./src/index.jsx
import React from 'react'
import ReactDOM from 'react-dom/client'
import Task from './views/Task'
//使用ANTD组件库
import { ConfigProvider } from 'antd' //配置库
import zhCN from 'antd/locale/zh_CN' //中文配置
import './src/index.less'

const root = ReactDOM.createRoot(document.getElementById('app'))
root.render(
	<ConfigProvider locale={ zhCN }>
        <Task />
    </ConfigProvider>
)
```

```jsx
// ./src/views/Task.jsx

import React, { useState, useEffect, useRef }from 'react'
import './Task.less'
import { Button, Form, Input, Modal, Table, Tag, Popconfirm, DatePicker, message } from 'antd'//你想用哪个组件就导入哪个
import { getTaskList, addTask, removeTask, completeTask} from '@api/index'

//对日期处理方法
const zero = function zero(text) {
    text = String(text)
    return text.length<2? '0'+text : text;
}
const formatTime = function formatTime(time){
    let arr = time.match(/\d+/g)
    let [,month,day,hours = '00',minutes = '00'] = arr
    return `${zero(month)}-${zero(day)} ${zero(hours)}:${zero(minutes)}`
}

const Task = function Task(){
    //表格列的数据
    const columns = [{
        title:'编号',
        dataIndex:'id',
        align: 'center',
        width: '8%',
    },{
        title:'任务描述', 
        dataIndex:'task',
        ellipsis: true,
        width: '50%',
    },{
        title:'状态',
        dataIndex:'state',
        align: 'center',
        width: '10%',
        render:text => {
        	return +text===1?'未完成' : '已完成'	
        }
    },{
        title:'完成时间',
        dataIndex:'time',
        align: 'center',
        width: '15%',
        render:(_, record) => {
            let {state, time, complete} = record
            if(+state === 2 ) time = complete
            return formatTime(time)
        }   
    },{
        title:'操作',
        render:(_,record) => {
            let { id, state } = record
            return(
            	<>
                	<Popconfirm title="您确定要删除此任务么？" 
                        onConfirm={handleRemove.bind(null,id)}>
                    	<Button type="link">删除</Button>
                	</Popconfirm>
                	{+state !==2? <Popconfirm title="您确定把此任务设置成完成吗？" 
                                    	onConfirm={handleUpdate.bind(null,id)}>
                    	<Button type="link">完成</Button>
                	</Popconfirm>:null}
            	</>
            )
        }
    }]
    //初始组件的状态
    let [selectedIndex, setSelectedIndex] =useState(0),
        [tableData, setTableData]=useState([]),
        [tableLoading, setTableLoading] = useState(false),
        [modalVisible, setModalVisible] = useState(false),
        [confirmLoading, setConfirmLoading] = useState(false),
        formIns = useRef(null)
    
    const query = async() => {
        try{
            let {code, list } await getTaskList(selectedIndex)
            if (+code !== 0 ) list = []
            setTableData(list)
        }catch(_){
            setTableLodaing(false)
        }
    }
    
    useEffect(() => {
       query() 
    },[selectedIndex])//每次依赖变化时，也会发生请求
        
    const closeModal = () => {
         setModalVisible(false);
        setConfirmLoading(false);
        formIns.resetFields();
    }
    
    const submit = async() => {
        try{
            await.formIns.validateFields()
            let {task,time} = formIns.getFieldsValue()
            time = time.format('YYYY-MM-DD HH:mm:ss')
            //表演校验通过，向服务器发送请求
            setConfirmLoading(true)
            let {code} =await addTask(task,time)
            if (+code === 0){
                closeModal()
                query()
                message.success('恭喜您，操作成功了')
            }else{
                message.error('很遗憾，操作失败了')
            }
        }catch(_){ }
    }
    
    const RemoveHandle = (id) => {
        try{
            let { code } = await removeTask(id)
            if(+code === 0){
                query()
                message.success('恭喜您，操作成功了')
            }else{
                
            }
        }catch(_){
            message.error('很遗憾，操作失败了')
        }
    }
    const handleUpdate = (id) => {
        try{
            let { code } = await completeTask(id)
            if(+code=== 0){
                query()
                message.success('恭喜您，操作成功了')
            }else{
                
            }
        }catch(_){
            message.error('很遗憾，操作失败了')
        }
    }
    render(){
        console.log('RENDER')
        let { tableData, tableLoading, modalVisible, confirmLoading, selectedIndex } = this.state;
        <div className="task-box">
            <div className="header">
                <h2 className='title'>TASK OA 系统任务管理系统</h2>
                <Button type="primary" onClick={()=>{
                        setModalVisible(true)
                        })
                    }}>新增任务</Button>
            </div>
            <div className='tag-box'>
                {['全部','未完成','已完成'].map((item, index) => {
                    return (
                        <Tag key={ index } 
                             color={ selectedIndex === selectedIndex? '#1677ff' : '' } 
                             onClick={()=>{
                                if(index === selectedIndex) return  //这一行可加可不加，react会自己判断，状态不会更新，视图也不会渲染。
                                setSelectedIndex(index)
                                query()
                            }}
                               >{ item }</Tag>
                           )
                })}             
            </div>
            
            <Table dataSource={ tableData } 
                columns={ columns } 
                loading={ tableLoading } 
                pagination={ false } 
                rowKey='id' />
            
            <Modal title="新增任务窗口" 
                open={ modalVisible } 
                confirmLoading={ confirmLoading } 
                keyboard= { false } 
                maskClosable={ false } 
                okText='确认提交' 
                onCancel={ closeModal } 
                onOk={ submit }>
                <Form layout="vertical" 
                    initialValues={{task: '',time:''}} 
                    validateTigger="onBlur" 
                    ref = { formIns }>
                	<Form.Item ref={x => this.formInx = x} label="任务描述" name="task" rules={[
                            {require: true,message:'任务描述必填'},
                            {min:6,message:'输入的内容至少6位及以上'}
                        ]}>
                        <Input.TextArea rows={ 4 }></Input.TextArea>
                    </Form.Item>
                    <Form.Item label="预期完成时间" name="time" rule={[
                            {require: true,message:'预期完成事件是必填项'}
                        ]}>
                        <DatePicker showTime />
                    </Form.Item>
                </Form>
            </Modal>
        </div>
    }
}
export default Task
```

> [!NOTE]
>
> 函数组件中，遇到：修改某个状态后（视图更新后），想去做一些事情，而这些事情，需要用到新的修改的状态值，
>
> 此时，我们不能直接在代码的下面编写，或者把状态改成同步的，这些都不可以，因为只有在函数重新执行，产生的新的闭包中，才可以获取最新的状态值。
>
> --->基于`useEffect`设置状态的依赖，在依赖的状态发生改变后，去做要做的事情。
>
> 修改某个状态后，想去做一些事情，但是要处理的事情，和新的状态值没有关系，此时可以把修改的状态的操作，基于`flushSync`变为同步处理即可

### 6、`useMemo`函数

```jsx
// ./views/Demo

import React, { useState, useMemo } from 'react'
import { Button } from 'antd'
import './Demo.less'

const Demo = function Demo(){
    let [supNum, setSupNum] = useState(10),
        [oppNum, setOppNum] = useState(5),
        [x, setX] = useState(0),
    
     
    //经过计算，算出支持比率
    //let total = supNum + oppNum,
    //    ratio = '--'
    //if(tital > 0) ratio = (supNum/total*100).toFixed(2) + '%'
    //函数组件每一次更新，都是把函数重新执行，产生一个新的闭包，内部代码也要重新执行一遍
    //如果我们修改的是其他状态值，视图更新了，此逻辑没必要重新执行了（如果此逻辑需要执行的事件比较长，一定会影响视图更新的速度）
    //诉求，在函数每一次重新执行的时候，如果此操作逻辑不应该去执行，只有依赖项发生改变，我们再去执行即可。
    
    //可以使用useMemo函数，上面函数换成下面执行
    let ratio = useMemo(()=>{
        let total = supNum + oppNum,
            ratio = '--'
        if(tital > 0) ratio = (supNum/total*100).toFixed(2) + '%'
        return total
    },[supNum,oppNum])
        
    return(
    	<div className='vote-box'>
            <div className='main'>
                <p>支持人数：{supNum}人</p>
                <p>反对人数：{oppNum}人</p>
                <p>支持比例：{ratio}</p>
                <p>x:{x}</p>
            </div>
            <div className='footer'>
                <Button type='primary' onClick={()=>{
                        setSupNum(supNum + 1)
                    }}>支持</Button>
                <Button type='primary' danger onClick={()=>{
                        setOppNum(oppNum + 1)
                    }}>反对</Button>
                <Button onClick={()=>{
                        setX(x+1)
                    }}>干点别的事</Button>
            </div>
        </div>
    )
}

export default Demo
```

> [!NOTE]
>
> ```
> let xxx = useMemo(callback, [dependencies])
> ```
>
> 第一次渲染组件的时候，`callback`会执行
>
> 后期只有依赖的状态值发生改变，`callback`才会再执行
>
> 每一次会把`callback`执行的结果赋值给`xxx`
>
> `useMemo`具备缓存的效果，再依赖的状态值没有发生改变，`callback`没有触发执行的时候，`xxx`获取的是上一次计算出来的结果

`useMemo`就是一个优化的Hook函数，如果函数组件中，有消耗性能和事件的计算操作，则尽可能用`useMemo`缓存起来，设置对应的依赖，这样可以保证，当非依赖的状态发生改变，不会去处理一些必要的操作，提高组件更新的速度。

### 7、`useCallback`函数

```jsx
// ./views/Demo

import React, { useState, useCallback } from 'react'
import { Button } from 'antd'
import './Demo.less'

let prev
const Demo = function Demo(){
    let [x, setX] = useState(0),

    //const handle = () => {}//每次调用函数，handle都会创造新的地址，比如第一次：0x001，第二次：0x002
    
    //因此可以使用useCallback函数，如下
    const handle = useCallback(()=>{
        
    })
    //以下用于验证两次函数是不是同一个
	if (!prev){
        prev = handle
    }else{
        console.log(handle === prev);//结果为false，这个结果就是普通函数运行的结果，如果是使用useCallback,结果为true
    }
    //函数组件的每一次更新，都是把函数重新执行，产生一个新的闭包，在闭包中所有创建函数的操作，都会重新创建新的堆内存，也就是函数都会重新创建。
    return(
    	<div className='vote-box'>
            <div className='main'>
                <p>0</p>
            </div>
            <div className='footer'>
                <Button type='primary' onClick={()=>{
                        setX(x+1)
                    }}>累加</Button>
            </div>
        </div>
    )
}

export default Demo
```

> [!NOTE]
>
> ```js
> const xxx = useCallback(callback, [dependencies])
> ```
>
> 组件第一次渲染，`useCallback`执行，创建一个函数“`callback`”，赋值给`xxx`
>
> 组件后续每一次更新，判断依赖的状态值是否改变，如果改变，则重新创建新的函数堆，赋值给`xxx`，但是如果，依赖的状态没有更新（或者没有设置依赖“[]”），则`xxx`获取的一直是第一次创建的函数堆，不会创建新的函数出来
>
> 简单的来讲，`useCallback`可以保证，函数组件每一次更新，不再把里面的小函数重新重建，用的都是第一次创建的。

> [!WARNING]
>
> `useCallback`不要乱用，并不是所有组件内部的函数，都拿其处理会更好！
>
> 虽然减少了堆内存的开辟，但是`useCallback`本身也有自己处理逻辑和缓存机制，这个也消耗时间啊

- **什么时候用它呢**

父组件嵌套子组件，父组件要把一个内部的函数，基于属性传递给子组件，此时传递的这个方法，我们基于`useCallback`处理以下会更好

```jsx
// ./views/Demo

import React, { useState, useCallback } from 'react'
import { Button } from 'antd'
import './Demo.less'
//以下是类组件情形
class Child extends React.PureComponent {
    render(){
        console.log('child Render')
        return(
            <div>我是子组件</div>
        )
    }
}

//以下是函数组件的情形
const Child = React.memo(function Child(props){
    console.log('Child Render')
    return(
        <div>我是子组件</div>
    )
})

//父组件
//诉求：当父组件更新的时候，因为传递给子组件的属性仅仅是一个函数。特点：基本应该算是不变的，所以不想再让子组件也跟着更新！
	//第一条：传递给子组件的属性（函数），每一次需要相同的堆内存地址（是一致的），基于useCallback处理！！
	//第二条：在子组件内部也要做一个处理，验证父组件传递的属性是否发生改变，如果没有改变，则让子组件不能更新，有变化才需要更新，继承React.PureCompontent即可，在shouldCompontemntUpdate中堆新老属性做了浅比较！函数组件是基于React.memo函数，对新老传递属性做比较，如果不一致，才会把函数组件执行，如果一致，则不让子组件更新

const Demo = function Demo(){
    let [x, setX] = useState(0),
    const handle = useCallback(()=>{
        
    })
    return(
    	<div className='vote-box'>
            <div className='main'>
                <p>0</p>
            </div>
            <div className='footer'>
                <Button type='primary' onClick={()=>{
                        setX(x+1)
                    }}>累加</Button>
            </div>
        </div>
    )
}

export default Demo
```

### 8、自定义`Hooks`函数

自定义`Hook`：

​	作用：提取封装一些公共的处理逻辑

​	玩法：创建一个函数，名字需要是`useXxx`，后期就可以在组件中调用这个方法了

```jsx
import React, { useState, useEffect } from 'react'
import { Button } from 'antd'
import './Demo.less'

const usePartialState = function usePartialState(initalValue){
    let [state, setState] = useState(initalValue)
    // setState:不支持部分状态更改的
    // setPartial: 我们期望这个方法可以支持部分状态的更改
    const setPartial =function setPartial(partialState){
        setState({
            ...state,
            ...partialState
        })
        
    }
    return [state, setPartial]
}

//自定义hook，在组件第一次渲染完毕后，统一干点啥事情
const useDidMount = function useDidMount(title){
    if (!title) title = 'React'
    //基于React内置的Hook函数，实现需求即可
    useEffect(()=>{
        document.title = title
    },[])
}

const Demo = function Demo(){
    let [state, setPartial] = usePartialState({
        supNum:10,
        oppNum:5
    }),
	
    useDidMount('哈哈哈')
    const handle = (type) => {
        if (type === 'sup'){
            setPartial({
                supNum:state.supNum+1
            })
            return
        }
        setPartial({
            oppNum: supNum.oppNum + 1
        })
    }	
        
    return(
    	<div className='vote-box'>
            <div className='main'>
                <p>支持人数:{ state.supNum }人</p>
                <p>反对人数:{ state.oppNum }人</p>
            </div>
            <div className='footer'>
                <Button type='primary' onClick={handle.bind(null, 'sup')}>支持</Button>
                <Button type='primary' danger onClick={handle.bind(null, 'opp')}>反对</Button>
            </div>
        </div>
    )
}

export default Demo
```

### 9、组件通信

- **类组件**

父组件

```jsx
// ./src/views/Vote

import React from 'react'
import './vote.less'
import VoteMain from './VoteMain'
import VoteFooter from './VoteFooter'

class Vote extends React.Component {
    state = {
        supNum:10,
        oppNum:0
    }
    //设置为箭头函数，不论方法在哪执行，方法中的this永远都是Vote父组件的实例
    change = (type) =>{
        let { supNum, oppNum } = this.state
        if(type === 'sup'){
            this.setState({ supNum: supNum + 1 })
        }
        this.setState({ oppNum: oppNum + 1 })
    }
    
    render(){
        let { supNum, oppNum } = this.state;
        return(
        	<div className="vote-box">
                <div className="header">
                	<h2 className="title">React<h2>
                    <span className="num">{ supNum + oppNum }<span>
                </div>
                <VoteMain supNum={ supNum } oppNum={ oppNum }/>
                <VoteFooter change={this.change}/>
            </div>
        )
    }
}
                        
export default Vote
```



子组件"`main`"

```jsx
import React from 'react'
import ProTypes from 'prop-types'

class VoteMain extends React.Compontent{
    //属性校验
    static defaultProp = {
        supNum: 0,
        oppNum: 0
    }
    static propType = {
        supNum: PropTypes.number,
        oppNum: PropTypes.number
    }
    render(){
        let { supNum, oppNum } = this.props
        let ratio = '--'
        let total = supNum + oppNum;
        if (total > 0) ratio = (supNum/total *100 ).toFixed(2) + '%'
        return(
            <div className="main">
                <p>支持人数：{supNum}人</p>
                <p>反对人数：{oppNum}人</p>
                <p>支持比率: {ratio}</p>
            </div>
        )
    }
}

export default VoteMain
```



子组件"`footer`"

```jsx
import React from 'react'
import { Button } from 'antd'
import ProTypes from 'prop-types'

class VoteFooter extends React.PureComponent{  //这里基于PureComponent进行了优化，使其在父组件更新时，不再反复调用change方法
    //规则校验
    static defaultProp = {
    }
    static propType = {
		change:PropTypes.func.isRequired
    }
    render(){
        let { change } = this.props
        return(){
            <div className="footer">
                <Button type="primary" onClick={change.bind(null,'sup')}>支持</Button>
                <Button type="primary" danger onClick={change.bind(null,'opp')}>反对</Button>
            </div>
        }
    }
}

export default VoteFooter
```

> [!TIP]
>
> 父子组件通信
>
> 1、父亲想把信息传递给儿子 => 基于属性
>
> 2、儿子想改父亲的数据 => 父亲把修改自己数据的方法，基于”属性“传递给儿子，儿子执行方法即可！
>
> 3、父亲想把一些HTML结构传给儿子 => 基于属性中children插槽
>
> 4、父亲在调用儿子的时候，可以给儿子设置ref，以此获得儿子的实例（或者儿子中暴露的数据和方法）

> [!IMPORTANT]
>
> 父子组件通信
>
> 1、就是以父组件为主导，基于属性是实现通信
>
> ​	原因：只有父组件可以调用子组件，此时才可以基于属性，把信息传递给子组件。父组件基于属性，可以把信息传递给子组件；父组件基于属性插槽，可以把HTML结构传递给子组件；父组件把方法基于属性传递给子组件，子组件把属性的方法执行。
>
> 2、父组件基于ref获取子组件实例（或者子组件基于`useImperativeHandle`暴露的数据和方法）
>
> 我们调用`Antd`组件库中的组件，就是经典的父子组件通信

组件渲染的顺序：依赖于**深度优先**原则

> 父组件第一次渲染
>
> 父组件`willMount` -> 父 `render` （子`willMount` -> 子`render` -> 子`didMount`） ->父`didMount`
>
> 父组件更新
>
> 父`shouldUpdate` -> 父`willUpdate` -> 父`render` （子`willReciveProps` -> 子`shouldUpdate` -> 子`willUpdate` -> 子`render` -> 子`didUpdate`）-> 父 `didUpdate`
>
> 我们完全可以在子组件内部做优化处理，验证传递的属性值有没有变化，如果没有变化，则禁止更新
>
> 父组件释放
>
> 父`willUnmount` -> 父释放中（子`willUnmount` -> 子释放） -> 父释放

- **函数组件**

父组件

```jsx
// ./src/views/Vote

import React,{ useState, useCallback } from 'react'
import './vote.less'
import VoteMain from './VoteMain'
import VoteFooter from './VoteFooter'

const Vote = function Vote {
    let [supNum, setSupNum] = useState(10),
        [oppNum, setOppNum] = useState(0)
    
    const change = useCallback((type)=>{
        if(type === 'sup'){
            setSupNum(supNum + 1 )
            return
        }
        setOppNum(oppNum + 1 )
    },[supNum, oppNum])//如果没有设置依赖，则函数永远是在第一次组件渲染，产生的闭包中创建，函数中用到的信息永远是第一次闭包中的信息//本次优化没有实际意义，仅供使用参考，
    
    return(
        <div className="vote-box">
            <div className="header">
                <h2 className="title">React<h2>
                <span className="num">{ supNum + oppNum }<span>
            </div>
            <VoteMain supNum={ supNum } oppNum={ oppNum }/>
            <VoteFooter change={change}/>
        </div>
    )
}

                   
export default Vote
```



子组件"`main`"

```jsx
import React,{ useMemo } from 'react'
import ProTypes from 'prop-types'

const VoteMain = function VoteMain(props){
    let { supNum, oppNum } = this.props
    // 基于useMemo实现复杂逻辑的"计算缓存"
    let ratio = useMemo(()=>{
        let ratio = '--'
        let total = supNum + oppNum;
        if (total > 0) ratio = (supNum/total *100 ).toFixed(2) + '%'
        return ratio
    })
    return(
        <div className="main">
            <p>支持人数：{supNum}人</p>
            <p>反对人数：{oppNum}人</p>
            <p>支持比率: {ratio}</p>
        </div>
    )
}

//属性校验
VoteMain.defaultProp = {
    supNum: 0,
    oppNum: 0
}
VoteMain.propType = {
    supNum: PropTypes.number,
    oppNum: PropTypes.number
}

export default VoteMain
```



子组件"`footer`"

```jsx
import React,{ memo } from 'react'
import { Button } from 'antd'
import ProTypes from 'prop-types'

const VoteFooter = function VoteFooter(){

    let { change } = props
    return(){
        <div className="footer">
            <Button type="primary" onClick={change.bind(null,'sup')}>支持</Button>
            <Button type="primary" danger onClick={change.bind(null,'opp')}>反对</Button>
        </div>
    }
};
//属性校验规则
VoteFooter.defaultProps={};
VoteFooter.propTypes = {
    change:PropTypes.func.isRequired
};
export default memo(VoteFooter)
```

### 10、`useContext`函数

**类组件的使用方法**

第一步：创建一个上下文对象，用来管理上下文信息

```jsx
// ./src/ThemeContext.js
import React from 'react'

const ThemeContext = React.createContext();
export default ThemeContext
```

第二步：让祖先组件Vote，具备状态和修改状态的方法，同时还需要把这些信息，储存到上下文中

```jsx
// ./src/views/Vote

import React from 'react'
import './vote.less'
import VoteMain from './VoteMain'
import VoteFooter from './VoteFooter'
import ThemeContext from '../ThemeContext'

class Vote extends React.Component {
    state = {
        supNum:10,
        oppNum:5
    }
    change = (type) =>{
        let { supNum, oppNum } = this.state
        if(type === 'sup'){
            this.setState({ supNum: supNum + 1 })
            return
        }
        this.setState({ oppNum: oppNum + 1 })
    }
    
    render(){
        let { supNum, oppNum } = this.state;
        return(
            <ThemeContext.Provider value={{  //基于上下文对象中，提供的Provider组件，用来向上下文存储信息：value属性指定的值就是要储存的信息，当祖先组件更新，render重新执行，会把最新的状态值，再次储存到上下文对象中
                    supNum,
                    oppNum,
                    change:this.change
                }}>
                <div className="vote-box">
                    <div className="header">
                        <h2 className="title">React<h2>
                        <span className="num">{ supNum + oppNum }<span>
                    </div>
                    <VoteMain />
                    <VoteFooter />
                </div>
            </ThemeContext.provider>
        )
    }
}
                        
export default Vote
```

第三步：在后代组件中，我们需要获取上下文信息

`VoteMain`：获取信息绑定即可

`VoteFooter`：获取信息，把获取的函数（修改组件状态的函数），再点击支持/反对的时候执行

```jsx
import React from 'react'
import ProTypes from 'prop-types'
import ThemeContext from '../ThemeContext'

class VoteMain extends React.Compontent{
    
    static contextType = ThemeContext
    render(){
        let { supNum, oppNum } = this.context
        let ratio = '--'
        let total = supNum + oppNum;
        if (total > 0) ratio = (supNum/total *100 ).toFixed(2) + '%'
        return(
            <div className="main">
                <p>支持人数：{supNum}人</p>
                <p>反对人数：{oppNum}人</p>
                <p>支持比率: {ratio}</p>
            </div>
        )
    }
}

export default VoteMain
```

> 导入创建的上下文对象
>
> 给类组件设置静态私有属性，`contextType` = 上下文
>
> 从`this.context`中获取需要的信息即可

```jsx
import React from 'react'
import { Button } from 'antd'
import ProTypes from 'prop-types'
import ThemeContext from '../ThemeContext'

class VoteFooter extends React.Component{

    render(){
        return(
            <ThemeContext.Consumer>
                {context => {
                    let { change } = context
                    <div className="footer">
                        <Button type="primary" onClick={change.bind(null,'sup')}>支持</Button>
                        <Button type="primary" danger onClick={change.bind(null,'opp')}>反对</Button>
                    </div>          
                }}
            </ThemeContext.Consumer>
        )
    }
}

export default VoteFooter
```

> context中，储存了上下文中的所有信息
>
> 返回内容就是我们渲染的视图
>
> 获取祖先上下文的信息
>
> ```jsx
> <ThemeContext.Consumer>
>     {context => {
>         let { XXX } = context
>         <div ></div>          
>     }}
> </ThemeContext.Consumer>
> ```

**函数组件的使用方法**

```jsx
// ./src/views/Vote

import React,{ useState } from 'react'
import './vote.less'
import VoteMain from './VoteMain'
import VoteFooter from './VoteFooter'
import ThemeContext from '../ThemeContext'

const Vote = function Vote {
    let [supNum, setSupNum] = useState(10),
        [oppNum, setOppNum] = useState(5)
    
    const change = (type)=>{
        if(type === 'sup'){
            setSupNum(supNum + 1 )
            return
        }
        setOppNum(oppNum + 1 )      
    
    return(
        <ThemeContext.Provider value={{
                supNum,
                oppNum,
                change
            }}>
            <div className="vote-box">
                <div className="header">
                    <h2 className="title">React</h2>
                    <span className="num">{ supNum + oppNum }<span>
                </div>
                <VoteMain />
                <VoteFooter />
            </div>
        </ThemeContext.Provider>
    )
}
              
export default Vote
```

方法一：使用`XXXContext.Consumer`解决

```jsx
import React from 'react'
import ProTypes from 'prop-types'
import ThemeContext from '../ThemeContext'

const VoteMain = function VoteMain(){
    return(
        <ThemeContext.Consumer>
            {context => {
                let { supNum, oppNum } = context
                <div className="main">
                    <p>支持人数：{supNum}人</p>
                    <p>反对人数：{oppNum}人</p>
                </div>
            }}
        </ThemeContext.Consumer>
    )
}

export default VoteMain
```

方法二：使用`useContext`函数

```jsx
import React,{ useContext } from 'react'
import ProTypes from 'prop-types'
import ThemeContext from '../ThemeContext'

const VoteMain = function VoteMain(){
    let {supNum,oppNum } = useContext(ThemeContext)
    return(
        <div className="main">
            <p>支持人数：{supNum}人</p>
            <p>反对人数：{oppNum}人</p>
        </div>
    )
}

export default VoteMain
```

### 11、React样式私有化处理

以下样式存在问题，各组件之间的样式，可能会冲突，所以，如何保持各组件之间的样式不冲突，则是组件式开发中，都需要考虑的问题

解决方法：样式私有化

```jsx
// ./src/index.jsx
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App'
//使用ANTD组件库
import { ConfigProvider } from 'antd' //配置库
import zhCN from 'antd/locale/zh_CN' //中文配置
import './src/index.less'

const root = ReactDOM.createRoot(document.getElementById('app'))
root.render(
	<ConfigProvider locale={ zhCN }>
        <App />
    </ConfigProvider>
)
```

- `app`组件

```jsx
// ./src/app

import React from 'react'
import Nav from './views/Nav'
import Menu from './views/Menu'

const App = function App(){
    return(
    	<div className="home-box">
            <Nav />
            <Menu />
            <div className="box">我是内容</div>
        </div>
    )
}
export default App
```

- `Nav`组件

```jsx
// ./src/views/Nav

import React from 'react'

const Nav = function Nav(){
    return(
    	<nav className='box'>
            <h2 className="title">购物商城</h2>
            <div className='list'>
                <a herf=''>首页</a>
                <a herf=''>秒杀</a>
                <a herf=''>我的</a>
            </div>
        </nav>
    )
}
export default Nav
```

```less
// ./src/views/Nav.less
.box{
    background: lightblue;
    
    .title{
        font-size: 20px;
        color: lightcoral;
    }
    
    .list{
        font-size: 14px;
        a{
            color: #000;
        }
    }
}
```

- `Menu`组件

```jsx
// ./src/views/Menu

const Menu = function Menu(){
    return(
        <div className="box">
        	<ul className="list">
                <li>手机</li>
                <li>电脑</li>
                <li>家电</li>
            </ul>
        </div>
    )
}

export default Menu
```

```less
// ./src/views/Menu.less

.box {
    background: lightpink;
    
    .list{
        font-size: 16px;
    }
}
```

- 方案一：内联样式（基础方案）

  - 每个组件的样式，不再基于样式类名处理了，而是使用内联式（行内样式）
  - 这样确实可以保证组件和组件之间，元素和元素之间样式不会冲突
  - 缺点：
    - 不利于样式的复用，如果提取为一个通用样式对象，则没有代码提示
    - 不能使用伪类
    - 编译后的样式和结构混淆在一些，也不利于优化
  - 这种方式绝对不能成为项目的主流处理方式，但是偶尔有一些需求，我们可以基于这种方式处理

- 方案二：样式处理的技巧（大多数项目都在用）

  - 基于样式表、样式类名这样的方式，但是需要人为有意识，有规范的，规范样式冲突问题，
    - 首先保证每个组件最外层样式类名是不冲突的
    - 命名方案：路径+组件名 作为组件外层容器的名字，例如：`.person-table-list-box{}`
  - 后期组件内元素，其样式，都是基于less/sass/stylus嵌入到指定外层容器的类名之下去编写
  - 缺点
    - `Css`选择器都具有相同的全局作用域，很容易
    - 性能低：预编译语言的嵌套，可能带来就是超长的选择器前缀，性能低
    - 没有真正的动态样式：`CSS`表中难以实现动态设置样式

- 方案三：`CSS` `Modules`

  - 样式类名 `xxx.module.css`

  ```jsx
  import React from 'react'
  import sty from './nav.module.css'
  
  const Nav = function Nav(){
  	return(
  		<nav className = {sty.body}>
              <h2 className = {sty.title}>购物商城</h2>
              <div className = {sty.list}>内容</div>
  		</nav>
  	)
  }
  ```
  
  - 我们可以基于`CSS Modules`实现样式的私有化管理，我们的样式都写在`xxx.module.css`文件中，这样的文件是`css`文件，不能使用`less/sass/stylus`这样的预编译语言，
  - 我们在组件中，基于`ES6Module`模块规范引入进来，`import sty from './xxx.module.css'`，`sty`存储的就是一个对象，对象中包含着多个键值对，键就是我们之前再`css`中编写的样式类名，值就是`webpack`编译后的样式类名
  - 我们编写的`css`样式也会被编译，所有之前的样式，也都是编译为混淆后的类名，命名规则：组件名--样式类名--Hash值
  - 我们再组件中，所有元素的样式类，基于`sty.xxx`去操作
  - 全局样式`:global(.clearfix){clear:both;}`增加`:global`属性，继承属性 `.link{composes: list;color:lightcoral;}`增加`composes`

-  方案四：`React-JSS`

  - `JSS`是一个`CSS`创作工具，它允许我们使用`javascript`以声明式，无冲突和可复用的方式描述样式，`JSS`是一种新的样式策略，`React-JSS`是一个框架集成，可以再`React`应用程序中使用`JSS`，它是一个单独的包，所以不需要安装`JSS`核心，只需要`React-JSS`包即可，`React-JSS`使用新的`Hooks API`将`JSS`与`React`结合

    ```jsx
    import React from 'react'
    import { createUseStyles } from 'react-jss'
    //基于createUseStyles方法，构建组件需要的样式，返回结果是一个自定义的Hook函数！
    //对象中的每个成员就是创建的样式类名
    //可以类似与less等预编译语言的“嵌套语法”，给后代/伪类设置样式
    //自定义Hook执行，返回一个对象，对象包含
    	//我们创建的样式类名，作为属性名
    	//编译后的样式类名是唯一的，作为属性值
    	//{box: 'box-0-2-1'}
    
    const useStyles = createUseStyles({
        //设置box 就是样式类名
        box:{
            backgroundColor:'lightblue',
            width: '300px'
        },
        title:{
            fontSize:'20px',
            color:'red',
            '$:hover':{
                color:'green'
            }
        },
        list:props =>{ //2、对1中的props进行解析返回函数，进行编译
            return{
                '$ a':{
                    fontSize: props.size + 'px',
                    color: '#000'
                }
            }
        }
        list:{
            '$ a':{
                fontSize: props => props.size //3、对个别值进行props的解析
                color: '#000'
            }
        }
    })
    
    const Nav = function Nav(){
        const {box, title ,list} = useStyles({
            size:14,
            color:'green'//1、这里可以传入参数，参数可以通过props参数拿到对应的属性
        })
        return(
            <nav className={box}>
                <h2 className={title}>购物商城</h2>
                <div className={list}>
                    <a>内容1</a>
                </div>
            </nav>
        )
    }
    //===============================================================================
    //因为useStyles是Hooks组件，如果使用的类组件引用，不能使用Hooks函数，可以参考下列方法解决代理解决
    class Menu extends React.Component {
        render(){
            let { box, list } = this.props
            return(
            	<div className={box}>
                    <ul className={list}>
                        <li>手机</li>
                        <li>电脑</li>
                        <li>家电</li>
                    </ul>
                </div>
            )
        }
    }
    
    const ProxyComponent = function ProxyComponent(component) {
        return function HOC (props){
            let sty = useStyles()
            return <Component {...props} {...sty}>
        }
    }
                
    export default ProxyComponent(Menu)
    ```

    因为样式是写在`js`里面的，我们可以基于一些逻辑操作，实现样式动态化管理

- **HOC高阶组件处理**

  React高阶组件：利用JS中的闭包，柯理化函数，实现组件代理

  我们可以在代理组件中，经过业务逻辑的处理，获取一些信息，最后基于属性等方案，传递给我们要最终渲染的组件

  ```jsx
  // ./src/index.jsx
  import React from 'react'
  import Demo from './views/Demo'
  
  const App = function App(){
      return(
      	<div className='home-box'>
          	<Demo x={10} y={20} enable={true} />
          </div>
      )
  }
  ```

  

  ```jsx
  // ./src/views/Demo
  import React from 'react'
  
  const Demo = function Demo(props){
      return (
      	<div className='demo'>
          	内容
          </div>
      )
  }
  // 执行ProxyTest方法，传递一个组件进来Component
  const ProxyTest = function proxyTest(component){
      //Component -> Demo
      //HOC: higher-order-components
      return function HOC(props){
          let isUse = false
          console.log(props)  //{x:10,y:20,enable:true}
          //真实要渲染的是Demo组件，把获取的props要传递给Demo
          return<Component {...props} isUse={ false }/>
      }
  }
  
  export default ProxyTest(Demo)
  //把函数执行的返回结果（是一个组件），基于ES6Module规范导出，供App使用,在当前案例中，我们导出的是HOC
  ```

- 方案五：`styled-components`

  ```jsx
  // ./src/app
  
  import React from 'react'
  import Nav from './views/Nav'
  import Menu from './views/Menu'
  
  const App = function App(){
      return(
      	<div className="home-box">
              <Nav />
              <Menu />
              <div className="box">我是内容</div>
          </div>
      )
  }
  export default App
  ```

  ```jsx
  // ./src/views/Nav
  
  import React from 'react'
  import { NavBox } from './src/views/NavStyle.js'
  
  const Nav = function Nav(){
      return(
      	<NavBox size={16} hover={'#ffe58f'}>
              <h2 className="title">购物商城</h2>
              <div className='list'>
                  <a herf=''>首页</a>
                  <a herf=''>秒杀</a>
                  <a herf=''>我的</a>
              </div>
          </NavBox>
      )
  }
  export default Nav
  ```

  ```js
  // ./src/views/NavStyle.js
  import styled from 'styled-components'
  import { colorRed, colorBlue, titleSize } from './common'
  // 基于“styled.标签名" 这种方式编写需要的样式，样式要写在"es6模板字符串中"，返回并且导出的结果是一个自定义的组件
  //vscode添加插件 vscode-styled-components
  export const NavBox = styled.nav`
  	background-color: lightblue;
  	width:300px;
  	.title{
  		font-size:${titleSize};
  		color:${colorRed};
  		$:hover{
  			color:${colorBlue};
  		}
  	}
  `
  export const NavBarBox = styled.div.attrs(props => {
      return { //设置默认值.attrs(props=>...)
          size:props.size || 16
      }
  })`
  	line-height:40px;
  	a{
  		font-size:${props=>props.size};
  		color:#000;
  		margin-right: 10px;
  		
  		&:hover{
  			color: ${colorBlue}
  		}
  	}
  `
  ```

  ```js
  // ./src/views/common.js
  
  import styled from 'styled-components'
  //编写一些通用的样式
  export const colorRed = '#ff4d4f'
  export const colorBlue = '#1677ff'
  export const titleSize = '18px'
  
  export const CommonListBox = styled.ul`
  	box-sizeing:border-box;
  	padding: 10px;
  	border: 1px solid #999;
  	
  	li {
  		font-size:14px;
  		line-height:30px;
  		overflow:hidden;
  		white-space:nowrap;
  		text-overflow:ellipsis;
  		$:hover{
  			color: ${colorRed}
  		}
  	}
  `
  ```

  > [!IMPORTANT]
  >
  > `React`样式私有化处理方案总结
  >
  > 1、行内样式 `style={{...}}`
  >
  > >  不适用于普通样式/主流样式的编写，特殊的需求可以基于其处理，样式需要基于逻辑动态计算，想基于行内样式提高样式权重
  >
  > 2、认为有意识有规范的，保证组件最外层样式类名的唯一性，组件内部的元素，都嵌入到这个样式样式类中编写！
  >
  > > 依赖于人（不能100%保证样式类的唯一性）
  > >
  > > 可能还会有部分样式，因为选择权重问题，发生一些冲突
  >
  > ------------------以下方法原理：基于技术方法，让组件和元素的样式类名唯一----------------------------
  >
  > 3、`cssModules`
  >
  > > 本质上还是写样式表，所有编写的样式都是静态的，用起来不是那么的方便，不能再使用嵌套等操作了
  >
  > 4、`css-in-js`思想：把`css`写在`js`中，这样可以基于`js`逻辑实现，样式的动态管理，实现通用样式的封装
  >
  > > `React-jss`
  > >
  > > `styled-components`（更简单）



## 四、React生态

### 1、`Redux`基础

> 复合组件通信
>
> 1、父子通信（具备相同父亲和兄弟组件）：`prop`属性（基于`ref`）
>
> 2、祖先和后代（具备相同祖先的平行组件）：`context`上下文
>
> `redux/react-redux`
>
> 也是实现组件之间通信的技术（插件），不管任何类型的插件，都可以基于这种方法，实现组件通信，`redux`是一种公共状态管理方案
>
> 后期实战开发中，父子组件一般都是基于：`props/ref/redux`，其余组件的通信一般都是基于`redux`管理

`Redux`库和工具

- `React-Redux` 是官方的库，它让`React`组件与`Redux`有了交互，可以从`store`读取一些`state`，可以通过 `dispatch actions`来更新`store`
- `Redux` `Toolkit` 推荐编写`Redux`逻辑的方法，包括我们认为对于构建`Redux`应用程序必不可少的包和程序，简化大多数`Redux`任务，防止了常见错误，使得我们编写`Redux`更加容易
- `Redux` `DevTools`扩展 可以显示`Redux`储存中状态随时间变化的历史记录，允许我们调试应用程序

步骤

> 1、创建全局公共的容器，来储存各组件需要的公共信息
>
> > `const store = createStore([reducer])`
> >
> > 在创建的store容器中，存储两部分内容，公共状态：各组件需要共享/通信的信息，事件池：存放一些方法（让组件可以更新的方法），特点：当公共状态一旦发生改变，会默认立即通知事件池中的方法执行！！
> >
> > 这些方法的执行，主要目的就是让指定的组件更新，而组件已更新，就可以获取最新的公共状态信息进行渲染
>
> 2、在组件内部，获取公共状态信息，然后渲染
>
> > `store.getState() -> {supNum...,oppNum...}`
>
> 3、让“组件可以更新”的方法放在公共容器的事件池中！
>
> > `store.subscribe(函数)`
> >
> > 后期公共状态改了，事件池中的方法会按照顺序依次执行，也就是让对应的组件也更新，组件只要更新，就可以从store容器中获取最喜欢的状态渲染！
>
> 4、创建容器的时候，需要传递`reducer`
>
> > ```js
> > let initial = { ... }//初始状态值
> > const reducer = function reducer(state=initial,action){
> > 	//state容器中的状态
> > 	//action 派发的行为对象（必须具备type属性）
> >     switch(action,type)
> >         //根据传递的type值不同，修改不同的状态信息
> >         ...
> >     
> > 	return state //返回的信息会替换store容器中的公共状态
> > }
> > ```
> >
> > 修改公共容器中的状态，为什么不能直接去修改
> >
> > 基于dispatch派发，通知reducer执行
> >
> > 在reducer中实现状态的更新
>
> 5、派发任务，通知reducer执行修改状态
>
> > ```jsx
> > store.dispatch({
> > 	type:xxx
> > 	...
> > })
> > ```
>
> 除了`redux`这五步核心步骤之外，我们还需要一些其他的知识做配和！

为了在各个组件中，都可以把创建的`store`获取到，我们可以基于上下文方案，在`index.jsx`中，基于`ThemeContext.Provider`把创建的`store`放在上下文中，因为所有组件最后都是在`index.jsx`中渲染，所有组件都可以理解为`index.jsx`的后代组件，基于上下文方案，获取在上下文中存储的`store`就可以了

```js
// ./src/store/index
import { createStore} from 'redux'

//管理员：修改store容器中的公共状态
let inital = {
    supNum: 10,
    oppNum: 5
}

const reducer = function reducer(state = inital, action){
	//state：储存store容器中的公共状态（最开始没有的时候，赋值初始状态initial）
    //action： 每一次基于dispatch派发的时候，传递进来的行为对象（要求必须具备type属性，存储派发的行为标识）
    //为了接下来的操作中，我们操作state，不会直接直接修改容器中的状态（要等到最后return的时候，我们需要先浅克隆
    //state = { ... state}）
    //接下来我们需要基于派发的行为标识，修改store容器中的公共状态信息
    switch(action.type){
            case "vote_sup";
            	...//例如：state.supNum++
            	break;
            case "vote_opp";
            	...//例如：state.oppNum++
            	break;
            default;
    }
    return state
    //return的内容，会整体替换Store容器中的内容
}

//改写法位于组件内，这仅供演示
//store.dispatch(
//	{
//        type:"do",
//        step:10
//    }
//)

//创建store公共容器
const store = createStore(reducer)
export default store
```

> [!IMPORTANT]
>
> 第一次派发的时候，`state`没有值，会把`initial`的值赋给`state`：第一次派发是在`redux`内部派发的，`type`值与`case`中都不相同，传递的`action.type`不会和任何逻辑匹配，返回的`state`就是初始值`initial`。目的就是把初始值赋值给`stat`e。
>
> 第二次派发的时候，`state`就是第一次的返回的初始值，`action`就是由`store.dispatch`派发的行为，由组件提供

```jsx
// ./src/index.jsx
import React from 'react'
import ReactDOM from 'react-dom/client'
import Vote from './views/Vote'
//使用ANTD组件库
import { ConfigProvider } from 'antd' //配置库
import zhCN from 'antd/locale/zh_CN' //中文配置
import './src/index.less'
//Redux
import store from './store'
import { ThemeContext } from './ThemeContext'
const root = ReactDOM.createRoot(document.getElementById('app'))
root.render(
	<ConfigProvider locale={ zhCN }>
        <ThemeContext.provider value = {{
                store  //存入store对象，基于上下文对象的Provider，把创建的store放在祖先的上下文中
            }}>
        	<Vote />
        </ThemeContext.provider>
    </ConfigProvider>
)
```

```jsx
// ./src/ThemeContext.js

import React from 'react'

const ThemeContext = React.createContext()
export default ThemeContext
```



```jsx
// ./src/views/Vote

import React, { useContext, useState, useEffect } from 'react'
import ThemeContext from '../ThemeContext'

import './vote.less'
import VoteMain from './VoteMain'
import VoteFooter from './VoteFooter'

const Vote = function Vote {
	const { store } = useContext(ThemeContext)
    //获取容器中的公共状态
   	console.log(store.getState())
    
    //组件第一次渲染完毕后，会让组件更新的方法，放在store的事件池中
    let [ num, setNum ] = useState(0)
    const update = () => {
        setNum(num + 1)
    }
    useEffect(()=>{//每一次组件更新，都会把最新的创建的update放入事件池中，可以时刻保证update在上级上下文是最新的闭包，[num]是最新的状态
        //let unsubscribe = store.subscribe(让组件更新的方法)
        //把让组件更新的方法放在store的事件池中
        //返回的unsubscribe方法执行，可以把刚才放入事件池中的方法移除掉
       let unsubscribe = store.subscribe(update) 
       return () => {
       		unsubscribe()//把上一次组件释放的时候，把上一次放在事件池中的方法移除掉
       }
    },[num])
    
    return(
        <div className="vote-box">
            <div className="header">
                <h2 className="title">React<h2>
                <span className="num">{supNum + oppNum}<span>
            </div>
            <VoteMain />
            <VoteFooter />
        </div>
    )
}

                   
export default Vote
```

```jsx
// ./src/views/VoteMain
import React,{ useMemo,useContext } from 'react'
import ProTypes from 'prop-types'
import ThemeContext from '../ThemeContext'

const VoteMain = function VoteMain(props){
    const { store } = useContext(ThemeContext)
    //...
    // 基于useMemo实现复杂逻辑的"计算缓存"
    let ratio = useMemo(()=>{
        let ratio = '--'
        let total = supNum + oppNum;
        if (total > 0) ratio = (supNum/total *100 ).toFixed(2) + '%'
        return ratio
    })
    return(
        <div className="main">
            <p>支持人数：{supNum}人</p>
            <p>反对人数：{oppNum}人</p>
            <p>支持比率: {ratio}</p>
        </div>
    )
}

//属性校验
VoteMain.defaultProp = {
    supNum: 0,
    oppNum: 0
}
VoteMain.propType = {
    supNum: PropTypes.number,
    oppNum: PropTypes.number
}

export default VoteMain
///类组件方式====================================================================================
import React,{ useMemo } from 'react'
import ProTypes from 'prop-types'
import ThemeContext from './/ThemeContext'

class VoteMain extends React.Component {
    static contextType = ThemeContext
    
	render(){
        const { store }=this.context
        let { supNum, oppNum } = store.getState()//获取公共状态信息绑定
        
        return(
            <div className="main">
                <p>支持人数：{supNum}人</p>
                <p>反对人数：{oppNum}人</p>
            </div>
        )
    }
    
    componentDidMount(){
        const { store } = this.context;
        store.subscribe(() => {
            this.forceUpdate()  //使用forceUpdate根据store变化，强制更新即可
        })
    }
}
```

```jsx
// ./src/views/VoteFooter
import React,{ memo, useContext } from 'react'
import { Button } from 'antd'
import ProTypes from 'prop-types'
import ThemeContext from './/ThemeContext'

const VoteFooter = function VoteFooter(){
	const { store } = useContext(ThemeContext)

    return(){
        <div className="footer">
            <Button type="primary" onClick={()=>{
                    store.dispatch({
                        type:'vote_sup'
                    })
                }}>支持</Button>
            <Button type="primary" danger onClick={()=>{
                    store.dispatch({
                        type:'vote_opp'
                    })
                }}}>反对</Button>
        </div>
    }
};
//属性校验规则
VoteFooter.defaultProps={};
VoteFooter.propTypes = {
    change:PropTypes.func.isRequired
};
export default memo(VoteFooter)
```

### 2、`Redux`源码分析

```js
// ./src/myRedux
//实现Redux的部分源码
import _ from './assets/utils'

const createStore = function createStore(reducer){
    if(typeof reducer !== 'function') throw new typeError('reducer not to be a function')

    let state,//存放公共状态
        listeners = [];//事件池
    //获取公共状态的方法
    const getState = function getState(){
        //返回公共信息即可
        return state
    }
    //向事件池中加入组件更新的方法
    const subscribe = function sunscribe(listener){
        //规则校验
        if(typeof listener !== 'function') throw new TypeError('listerner is not function')
        //把传入的方法（让组件更新的方法）加入到事件池，需要做去重处理
        if(!listeners.includes(listener)){
        	listener.push(listener)
        }
        //返回一个从是事件池中，移除方法的函数
        return function unsubscribe(){
            let index = listeners.indexOf(listener)
            listeners.splice(index, 1)
        }
    }
    //派发任务通知Reducer执行
    const dispatch = function dispatch(action){
        //规则校验
        if(!_.isPlainObject(action)) throw new typeError('action must be plain onjects')
        if(typeof action.type === 'undefined') throw new typeError('action may not have an undefined type property')
        //把reducer执行,传递：公共状态，行为对象。，接收执行的返回值，替换公共状态
        state = reducer(state，action)
        
        //当状态更改，我们还需要把事件池中的方法执行
        linteners.forEach(listener => {
            listener()
        })
        
        return action
    }  
    
    //redux内部会默认进行一次dispatch派发，目的就是给公共容器中的状态赋值初始值
	dispatch({
        type:symbol()
    })
    //返回创建的store对象
    return {
        getState,
        subscribe,
        dispatch
    }
    
}
```

### 3、`Redux`操作流程

- 为什么不直接修改store里面的公共状态，而是采用对象全覆盖的方式

  如果直接修改，那么修改的逻辑是写在各个组件中的

  1、不利于逻辑的复用

  2、不利于维护和管理

**第一步**：创建store容器

```js
// ./src/store/index
import { createStore } from 'redux'

const initialState = {
    //...
    item:'项目1'
    //...
}
const reducer = function reducer(state = initialState, action){
    state =deepClone(state)//deepClone为自造函数，用于深克隆，deepClone需要自己写，这里直接引用名字
    
    switch(action.type){
        case "操作1" :
            //...(修改行为)
            break;
        case "操作2" :
            //...(修改行为)
            break;
       	default;
    }
    
    return state
}

const store = createStore(reducer)
export default store
```

**第二步**：使用`React.createContext()`函数创建全局数据通信组件

```js
// ./src/ThemeContext.js
import React from 'react'

const ThemeContext = React.creatContext()
export default ThemeContext
```

**第三部**：将`store`对象通过组件的`XxxContext.Provider`函数共享到全局

```jsx
// ./src/index.jsx
import React from 'react'
import ReactDOM from 'react-dom/client'
import Item from './views/Item'

//导入redux需要的组件
import ThemeContext from './src/ThemeContext'
import store from './store/index'

const root = ReactDOM.createRoot(document.getElementById('app'))
root.render(
    <ThemeContext.Provider value={{store}}> {/*通过value进行赋值共享*/}
        	<Item />
    </ThemeContext.Provider>
)
```

**第四步**：在组件中导入通过`useContext`函数导入`store`，再通过`store`解构出变量供组件使用

函数组件的操作方式

```jsx
// ./src/views/Item
import ThemeContext from './src/ThemeContext'
import React, {useContext, useEffect, useState} from 'react'
import Button from './src/views/Button'

const Item = function Item(){
	const { store } = useContext(ThemeContext)
    const { item, ... } = store.getState()
    
    //视口更新的办法：例如：let [, forceUpdate] = useState(0)
    useEffect (()=>{
        store.subscribe(()=>{
            //视口更新的方法
            //forceUpdate(+new Date())
        })
    },["视口跟新的依赖(视情况添加，示例方法可不用加)"])
    return(
    	<div className="itemName">
            { item }
        </div>
        <Button />
    )
}

export default Item
```

类组件的操作方式

```jsx
// ./src/views/Button
import ThemeContext from './src/ThemeContext'
import React from 'react'
import Button from './src/views/Button'

class Item extends React.Compontent(){
	static contextType = ThemeContext
    
    render(){
        const { store } = this.context
        const { item, ... } = store.getState()
        
        return(
            <div className="itemName">
                { item }
            </div>
            <Button />
        )
    }
    
    compontentDidMount(){  //在组件卸载的时候，通过store订阅的方式，监控store的state变化，一旦state变化，通知订阅函数（subscribe）更新指定函数，这里的指定函数应该是可以让视图更新的方法，在类组件中，通常使用forceUpdate进行强制视口更新
        const { store } = this.context
        store.subscribe(()=>{
            this.forceUpdate()
        })
    }
}

export default Item
```

将方法同步到行为组件中

```jsx
// ./src/views/Button
import react, {useContext, useState} from 'react'
import ThemeContext from './src/ThemeContext'

const Button = function Button(){
    const { store } = useContext(ThemeContext)
    const { item, ... } = store.getState()
    const handleClick = () =>{
        store.dispatch({
            //...
            type: "操作1"//必须项
            //...
        })
    }
    
    return(
    	<>
        	<button onClick={ handleClick }>按钮</button>
        </>
    )
}
```

### 4、`reducer`的拆分与合并

在真正的项目中，我们一定会把状态和reducer的管理，按照模块化进行划分

- reducer工程化开发
  - 按照模块，把reducer进行单独管理，每个模块都有自己的reducer；最后我们还要把所有的reducer合并，合并成一个，赋值给我们创建的store

**总的`reducer`**

```jsx
//新建reducers文件夹，新建index文件，合并各个模块的reducer
//合并各个模块的reducer，最后创建出一个总的reducer,使用combineReducers
// ./src/reducers/index
import { combineReducers } from 'redux'
import voteReducer from './voteReducer'
import personalReducer from './personalReducer'

const reducer = combineReducers({
    vote: voteReducer,
    personal: personalReducer
})

//在其他地方引用的reducer中的state按照自己命名的方式进行管理
//store.getState().vote才是vote中的对象
//state = {
//	vote:{
//        val:'变量1'
//    },
//  personal:{
//        val:'变量1'
//    }
//}
```

**分的`reducer`**

```js
//在reducers文件夹下，新建personalReducer文件
// ./src/reducers/personalReducer

const initialState = {
    val:'变量1'
    ...
}

const personalReducer = function personalReducer(state= initialState, action){
    state=deepClone(state)  //这里的deepclone为深克隆函数，需要自己写，这里仅引用名称，表示这里已经进行深克隆处理了
    switch(state.type){
        case "项目1":
            //操作val
            break;
        case "项目2":
            //操作val
            break;
        ...
        default;
    }
    
    return state
}

export default personalReducer
```

**分的`reducer`**

```js
//在reducers文件夹下，新建voteReducer文件
// ./src/reducers/voteReducer

const initialState = {
    val:'变量1'
    ...
}

const voteReducer = function voteReducer(state= initialState, action){
    state=deepClone(state)  //这里的deepclone为深克隆函数，需要自己写，这里仅引用名称，表示这里已经进行深克隆处理了
    switch(state.type){
        case "项目1":
            //操作val
            break;
        case "项目2":
            //操作val
            break;
        ...
        default;
    }
    
    return state
}

export default voteReducer
```

**`store`中引用**

```jsx
// ./src/store/index
import { createStore } from 'redux'
import reducer from './reducer'

const store = createStore(reducer)
export default store
```

**组件中使用**

状态的获取

```jsx
// ./src/views/Item
import ThemeContext from './src/ThemeContext'
import React, {useContext, useEffect, useState} from 'react'
import Button from './src/views/Button'

const Item = function Item(){
	const { store } = useContext(ThemeContext)
    const { item, ... } = store.getState().vote//对应的名字
    //...
}
```

方法的派发

```jsx
// ./src/views/Button
import react, {useContext, useState} from 'react'
import ThemeContext from './src/ThemeContext'

const Button = function Button(){
    const { store } = useContext(ThemeContext)
    const { item, ... } = store.getState()
    const handleClick = () =>{
        store.dispatch({
            //...
            type: "操作1"//必须项
            //...
        })
    }
	//派发任务不需要进行任何改变，还是按照以前的用法进行派发，此时派发会依次遍历事件池中的所有事件（不管是哪个模块），匹配上后才执行
}
```

### 5、派发行为标识宏管理

不管哪个模块，哪个组件，我们派发的标识必须是唯一的

基于宏管理，让所有派发的行为标识，具备唯一性

```js
在store下，创建action-types.js
|-store
	|-action
		|-index.js
	|-reducers
		|-index/js
		|-partReducer.js
		|-...
```

```js
//统一管理需要派发的行为标识
// .src/store/action-types
	//为了保证不冲突，我们一般都是这样命名的，模块名——派发的行为标识（大写）
	//变量和储存的值是一致的
	//所有需要派发的行为标识，都在这里定义

export const VOTE_SUP = 'VOTE_SUP'
export const VOTE_OPP = 'VOTE_OPP'
//...
```

```jsx
// ./src/reducers/voteReducer
import * as TYPES from '../action-types'

const initialState = {
    val:'变量1'
    ...
}

const voteReducer = function voteReducer(state= initialState, action){
    state=deepClone(state)  //这里的deepclone为深克隆函数，需要自己写，这里仅引用名称，表示这里已经进行深克隆处理了
    switch(state.type){
        case TYPES.VOTE_SUP://这里统一对名称进行了管理，在store目录下的action-types
            //操作val
            break;
        case TYPES.VOTE_OPP:
            //操作val
            break;
        ...
        default;
    }
    
    return state
}

export default voteReducer
```

```jsx
// ./src/views/Button
import react, {useContext, useState} from 'react'
import ThemeContext from './src/ThemeContext'
import * as TYPES from '../action-types'

const Button = function Button(){
    const { store } = useContext(ThemeContext)
    const { item, ... } = store.getState()
    const handleClick = () =>{
        store.dispatch({
            //...
            type: TYPES.VOTE_SUP://这里统一对名称进行了管理，在store目录下的action-types
            //...
        })
    }

}
```

统一管理派发的行为标识，除了保证不冲突，还能避免程序员因为粗心大意，导致的错误！

### 6、`actionCreator`的创建

```js
//新创建actions文件夹，新建index.js文件
|-store
	|-action
		|-index.js
	|-reducers
		|-index/js
		|-partReducer.js
		|-...
```

```js
// ./src/actions/index.js
//把各个板块的action合并为一个action即可
import voteAction from './voteAction'
import personalAction from './personalAction'

const action = {
    vote: voteAction,
    personal: personalAction
}
export default action
```

```js
// ./src/actions/voteAction.js
//Vote板块要派发的行为对象管理，里面有很多方法，每一个方法执行，都返回要派发的行为对象
import * as TYPES from '../action-types'
const voteAction = {
    support(){
        return{
            type: TYPES.VOTE_SUP
        }
    },
    oppose(){
        return{
            type:TYPES.VOTE_OPP
        }
    }
}
export default voteAction
```

```js
// ./src/actions/personalAction.js
import * as TYPES from '../action-types'
const personalAction = {
    support(){
        return{
            //...
        }
    },
    oppose(){
        return{
            //...
        }
    }
}
export default personalAction
```

```jsx
// ./src/views/Button
import react, {useContext, useState} from 'react'
import ThemeContext from './src/ThemeContext'
import action from '../store/actions'

const Button = function Button(){
    const { store } = useContext(ThemeContext)
    const { item, ... } = store.getState()
    const handleClick = () =>{
        store.dispatch({
			action.vote.support()
        })
    }
	//...

}
```

> [!IMPORTANT]
>
> 此操作看似很麻烦，但有其实际意义，我们称之为 创建`actionCreator`，在我们接下来，处理`react-redux`的时候，非常方便

### 7、`combineReducers`的源码

```js
// myCombineReducers.js

const combineReducers = function combineReducers(reducers){
    //reduces是一个对象，以键值对存储，模块名&每个模块的reducer
    let reducerskeys = Reflect.ownKeys(reducers) //此操作就是将reducerskey下面的可枚举属性以列表的姓氏展示出来['vote','personal']
    
    //返回一个合并的reducer
    	//每一次dispatch派发，都是把这个reducer执行
    	//state就是redux容器中的公共状态
    	//action就是派发时候传进来的行为对象
    
	return function reducer(state = {},action){
        //把reducers中的每一个小的reducer执行
        //把对应模块的状态/action行为对象传递进来，返回的值替换当前模块下的状态！！
        let nextState = {}
        reducerskeys.forEach(key => {
            //key:'vote'/'personal'模块名
            let reducer = reducers[key]  //每一个模块的reducer对象
            nextState[key] = reducer(state[key],action)
        })
        return nextState
    }
}
```

### 8、`react-redux`的运用

`react-redux`最大的特点就是让`redux`的操作在`react`项目中更简单一些，主要是在组件应用的时候更方便一些

1、内部自己创建了上下文对象，并且我们可以把`store`放在上下文中，在组件中使用的对象的时候，无需我们自己再获取上下文中`store`了，它可以帮我们获取到

2、在组件中，我们向获取公共状态信息进行绑定等，无需我们自己基于上下文对象获取`store`，也无需自己基于`getState`获取公共状态，直接基于`react-redux`提供的`connect`函数处理即可！而且，也不需要我们手动把组件更新的方法，放在事件池中了，`react-redux`内部帮我们处理了！！

```js
|-assets
|-store
|-src
	|-views
		|-Vote.jsx
		|-Vote.less
		|-VoteFooter.jsx
		|-VoteMain.jsx
	|-index.jsx
	|-index.less
```

```jsx
// ./src/index.jsx
import React from 'react'
import ReactDOM from 'react-dom/client'
import Item from './views/Item'

//导入redux需要的组件
import store from './store/index'
import { Provider } from 'react-redux'  //**新增行

const root = ReactDOM.createRoot(document.getElementById('app'))
root.render(
    <Provider store={ store }>  {/*修改行*/}
        	<Item />
    </Provider>
)
```

```jsx
// ./src/views/Item
import React, {useContext, useEffect, useState} from 'react'
import Button from './src/views/Button'

import { connect } from 'react-redux'//*新增行

const Item = function Item(props){
	let { item } = props
    return(
    	<div className="itemName">
            { item }
        </div>
        <Button />
    )
}

export default connect(state=>state.item)(Item) //*修改行
//这里的state.item中的item是state中的state键名，由自己命名，
//connetc(...)(Item)中的Item是该组件的组件名，注意区分

//语法：connect(mapStateToProps,mapDispatchToProps)(这里放我们要渲染的组件)
//mapStateToProps：可以获取到redux中的公共状态，把需要的信息当作属性，传递组件即可
//connect(state=>{
	//state是储存redux容器中，所有模块的公共状态信息
	//返回对象中的信息，就是要作为属性，传递给组件的信息
	//return{
		//supNum:state.vote.supNum
	//}
//})(Vote)

--------------------------------------------------------------------------------------
//以下是类组件

// ./src/views/Item
import React from 'react'
import Button from './src/views/Button'

import { connect } from 'react-redux'//*新增行

class Item extends React.Compontent{
	render(){
        let { item } = this.props
        return(
            <div className="itemName">
                { item }
            </div>
            <Button />
        )
    }
}

export default connect(state=>state.item)(Item) //*修改行
```

```jsx
// ./src/views/Button
import React from 'react'
import { connect } from 'react-redux'
import action from '../store/actions'

const Button = function Button(props){
	let { support, oppose} = props
    const handleClick = () =>{
		support
    }

}

export default connect(
    				null,
                    dispatch =>{
    					return{
        					support(){
                                dispatch(action.vote.support())
                            },
        					oppose(){
                                dispatch(action.vote.oppose())
                            }
    					}
					}
                      )(Button)
//简单写法，以上为标准写法
//export default connect(null,action.vote)(Button)
//语法：connect(mapStateToProps,mapDispatchToProps)(这里放我们要渲染的组件)
//mapDispatchToProps：把我们需要派发的任务，当作属性传递给组件
//connect(
//    null,
//    dispatch=>{
//        //dispatch:store.dispatch 派发任务的方法
//        
//        //返回对象中信息，会作为属性传递给组件
//        return{
//            ...
//        }
//    })(Button)
```

```js
// ./src/actions/index.js
//把各个板块的action合并为一个action即可
import voteAction from './voteAction'
import personalAction from './personalAction'

const action = {
    vote: voteAction,
    personal: personalAction
}
export default action
```

### 9、`react-redux`的使用总结

react工程化处理

1、把reducer状态按照模块进行划分和管理，把所有模块的reducer合并为一个即可
 ```js
 |-store
 	|-actions
 		|-index.js
 		|-xxXAction.js
 		|-xxXAction.js
 		|-...
 	|-reducers
 		|-index.js
 		|-xxXReducer.js
 		|-xxXReducer.js
 		|-...
 	|-action-types.js
 	|-index.js
 ```

```js
// ./reducers/index
import { combineReducers } from 'redux'
import xxxReducer from './xxxReducer'
import xxxReducer from './xxxReducer'
//...

const reducer = combineReducers({
    partA: xxxReducer,
    partB: xxxReducer,
    partC: ...
})
```

```js
// ./reducers/xxxReducer

const initialState = {
    val:'变量1'
    ...
}

const xxxReducer = function xxxReducer(state= initialState, action){
    state=deepClone(state)  //这里的deepclone为深克隆函数，需要自己写，这里仅引用名称，表示这里已经进行深克隆处理了
    switch(state.type){
        case "项目1":
            //操作val
            break;
        case "项目2":
            //操作val
            break;
        ...
        default;
    }
    
    return state
}

export default personalReducer
```

 2、每一次任务派发都会把所有模块的reducer，依次去执行，派发时候传递的行为（对象标识）是统一的，所以我们要保证各个模块之间，派发行为标识它的唯一性！派发行为标识的宏管理

```js
// action-types.js
export const XXX_XX = 'XXX_XX'
export const XXX_XX = 'XXX_XX'
//...
```

3、创建`actionCreator`对象，按照模块管理我们需要派发的行为对象

```js
// ./actions/index.js
import xxxAction from './xxxAction'
import xxxAction from './xxxAction'
...

const action = {
    partA: xxxAction,
    partB: xxxAction,
    partC:...
}
export default action
```

```js
// ./actions/xxxAction.js
import * as TYPES from '../action-types'

const xxxAction = {
    xxx(){ //行为1
        return{
            type: TYPES.XXX_XX
        }
    },
    xxx(){ //行为2
        return{
            type:TYPES.XXX_XX
        }
    }
}
export default xxxAction
```

4、最后合并到入口文件

```js
// index.js
import { createStore } from 'redux'
import reducer from './reducer/index'

const store = createStore(reducer)
export default store
```



> [!TIP]
>
> 如果我们使用的是`redux`：
>
> 1、我们需要创建上下文对象，基于其Provider把创建的store放在根组件的上下文信息中，后代组件需要基于上下文对象，获取上下文中的store
>
> 2、需要用到公共状态的组件
>
> > `store.getState()`获取公共状态
> >
> > `store.subscribe()`让组件更新函数放在事件池中
>
> 3、需要派发的组件
>
> > `store.dispatch(actionCreator)`



> [!TIP]
>
> 如果我们使用的是`react-redux`：
>
> 1、提供的Provider组件，可以自己在内部创建上下文对象，把store放在跟组件上下文中
>
> 2、提供的connect函数，在函数内部，可以获取到上下文中的store，然后快速的把公共状态，以及需要派发的操作，基于属性传递给组件！
>
> > `connect(mapStateToProps, mapDispatchToProps)（需要渲染的组件）`



> [!IMPORTANT]
>
> 核心源码
>
> `createStore`
>
> `combineReducers`
>
> `bindActionCreators`
>
> `redux`在设计上，存在一些不好的地方，
>
> > 我们基于`getState()`获得的数据，是直接和`redux`中的公共状态，共有相同的地址，这样导致，是可以直接修改公共状态信息的，优化操作，我们应该在返回的时候，对数据做深拷贝处理。
> >
> > 我们会把让组件更新的办法，放在事件池中，当公共状态改变，会通知事件池中的所有方法执行，此操作，放置方法的时候，没有办法设置状态的依赖，这样，后期不论哪个状态被修改，事件池中的所有方法都要执行（相关组件都要进行更新），优化方法：我们向事件池中加入方法的时候，把依赖的信息也设置了，在每一次执行`reducer`修改状态之前，把之前的状态储存一份`prev`，修改后的最新状态也获取到`next`，在后期通知事件中方法执行的时候，拿出来某个方法是否执行，就可以`prev`和`next`中是否改变。
> >
> > 所有`reducer`的合并，其实不是代码的合并，而是创建一个总的`reducer`出来，每一次派发都会让总的`reducer`执行，而在这里，会把每个模块的`reducer`都完完整整执行一遍，（即便中间已经发现匹配的逻辑了，也会继续把其他模块中`reducer`执行）,优化方法：在某个模块的`reducer`中，如果派发的行为标识有匹配了，因为行为标识是统一管理的，所以遇到匹配的，说明后面不可能再匹配的，则停止执行后面的`reducer`

### 10、`react-redux`的源码解读




## 五、精品文章

### 1、`useEffect()` 用法总结

在 `React` 函数组件中，`useEffect`是处理 “副作用” 的核心工具。无论是数据请求、定时器管理，还是 `DOM` 操作，都离不开它。

**先搞懂：什么是 “副作用”？**

在 `React` 中，“副作用” 指的是**组件渲染过程之外的操作**—— 这些操作不直接参与 `UI` 渲染，但会影响组件或应用的状态。常见的副作用包括：

- 发送网络请求（获取数据）
- 操作 `DOM`（如自动聚焦输入框）
- 设置定时器、订阅事件（如 `WebSocket`）
- 手动修改页面标题

组件的主要任务是 “渲染 `UI`”（正作用），而副作用需要在渲染完成后执行，否则会阻塞渲染，影响页面性能。`useEffect`的作用就是**将副作用与渲染过程分离**，确保渲染优先，副作用在渲染完成后再执行。

```jsx
function App() {
  // 组件的主要任务：渲染UI
  return <div>Hello World</div>;
}
```

如果直接在组件函数中写副作用（比如网络请求），会导致这些操作在每次渲染时同步执行，可能阻塞 UI 更新。`useEffect`则能让副作用 “延后” 执行，不影响渲染效率。

**声明一个副作用  生命周期时机解析**

`useEffect`的基本语法很简单，包含三个核心部分：

```jsx
useEffect(() => {
  // 1. 副作用逻辑（如请求数据、设置定时器）
  console.log('执行副作用');

  // 2. 清理函数（可选，用于回收资源）
  return () => {
    console.log('清理副作用');
  };
}, [
  // 3. 依赖项数组（控制副作用何时执行）
]);
```

**（1）副作用逻辑：要执行的操作**

这部分是`useEffect`的核心，放需要执行的副作用代码。比如请求数据：

```jsx
useEffect(() => {
  // 副作用：请求接口数据
  const fetchData = async () => {
    const response = await fetch('https://api.example.com/data');
    const data = await response.json();
    console.log('获取到数据：', data);
  };
  fetchData();
});
```

**（2）清理函数：避免内存泄漏**

清理函数是`useEffect`返回的一个函数，用于**回收副作用产生的资源**，避免组件卸载后资源残留（如定时器未清除、网络请求未取消）。

```jsx
useEffect(() => {
  // 副作用：设置定时器
  const timer = setInterval(() => {
    console.log('定时器执行中...');
  }, 1000);

  // 清理函数：组件卸载或副作用重新执行前，清除定时器
  return () => {
    clearInterval(timer);
    console.log('定时器已清除');
  };
}, []);
```

**什么时候执行清理函数？**

- 组件卸载时（如页面跳转，组件被移除）
- 副作用因依赖项变化而重新执行前（先清理旧的副作用，再执行新的）

React 的函数组件不像 class 组件那样有明显的生命周期方法（如 `componentDidMount`、`componentDidUpdate`、`componentWillUnmount`）。
 `useEffect` 就是函数组件中处理这些生命周期的“代理”。 根据依赖项的不同，`useEffect` 有以下三种生命周期“行为”：

| 模式     | 依赖项    | 触发时机               | 对应生命周期                     |
| -------- | --------- | ---------------------- | -------------------------------- |
| 初始挂载 | `[]`      | 组件挂载完成后执行一次 | `componentDidMount`              |
| 依赖更新 | `[count]` | `count` 更新时执行     | `componentDidUpdate`             |
| 每次渲染 | 不写依赖  | 每次渲染后都执行       | `componentDidUpdate`（所有更新） |

**（3）依赖项数组：控制副作用的执行时机**

依赖项数组是`useEffect`最关键的部分，它决定了副作用 “什么时候执行”。

**① 无依赖项数组：每次渲染后都执行**

```jsx
useEffect(() => {
  console.log('每次渲染后都执行');
});
```

这种写法下，副作用会在**初始渲染完成后执行**，且**每次组件更新（重新渲染）后都会再次执行**。适合需要 “实时响应所有变化” 的场景（如根据最新状态更新 `DOM`）。

**② 空依赖项数组`[]`：仅在首次渲染后执行**

```jsx
useEffect(() => {
  console.log('仅在首次渲染后执行一次');
  // 常见场景：初始化数据请求、设置一次性事件监听
}, []);
```

空依赖项表示 “没有依赖任何状态 / 变量”，因此副作用只会执行一次（相当于类组件的`componentDidMount`）。这是最常用的写法之一，尤其适合 “只需要初始化一次” 的操作。

**③ 有依赖项的数组：依赖项变化时执行**

```jsx
const [userId, setUserId] = useState(1);

useEffect(() => {
  console.log(`userId变化为${userId}，重新执行副作用`);
  // 场景：根据userId重新请求用户数据
  fetch(`https://api.example.com/user/${userId}`);
}, [userId]); // 依赖userId，只有userId变化时才执行
```

当数组中的依赖项（如`userId`）发生变化时，副作用会重新执行（相当于类组件的`componentDidUpdate`）。这能精准控制副作用的执行时机，避免不必要的重复操作。

**常见问题**

(1)为什么 `useEffect` 不能直接用 `async`？

```jsx
// 错误写法
useEffect(async () => {
  const data = await fetchData(); // 直接用async会报错
}, []);
```

**原因**：`async`函数返回的是 `Promise` 对象，而`useEffect`要求回调函数返回 “清理函数” 或`undefined`。`Promise` 会被 `React` 视为 “无效的返回值”，导致警告。

**正确写法**：在内部定义 async 函数并调用：

```jsx
useEffect(() => {
  const fetchData = async () => { // 内部定义async函数
    const data = await fetchData();
  };
  fetchData(); // 执行
}, []);
```

**清理函数的执行时机**

清理函数不仅在 “组件卸载时” 执行，还会在 “副作用因依赖项变化而重新执行前” 执行。比如：

```jsx
jsx 体验AI代码助手 代码解读复制代码const [id, setId] = useState(1);

useEffect(() => {
  console.log(`副作用执行，当前id：${id}`);
  return () => {
    console.log(`清理函数执行，旧id：${id}`);
  };
}, [id]);
```

当`id`从 1 变为 2 时，执行顺序是：

1. 先执行旧的清理函数（打印 “旧 `id：1`”）
2. 再执行新的副作用（打印 “当前 `id：2`”）

这种机制确保每次副作用执行前，旧的资源已被清理，避免冲突。

**实战场景：`useEffect` 的典型用法**

数据请求：组件挂载后获取初始数据

```jsx
function UserList() {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    // 定义异步请求函数
    const getUsers = async () => {
      try {
        const response = await fetch('https://api.example.com/users');
        const data = await response.json();
        setUsers(data); // 存储数据到状态
      } catch (err) {
        console.error('请求失败：', err);
      }
    };

    getUsers(); // 执行请求
  }, []); // 空依赖，仅执行一次

  return (
    <ul>
      {users.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```

定时器管理：避免组件卸载后继续执行

```jsx
function Timer() {
  const [seconds, setSeconds] = useState(0);

  useEffect(() => {
    // 设置定时器，每秒更新秒数
    const timer = setInterval(() => {
      setSeconds(prev => prev + 1); // 函数式更新，不依赖当前seconds
    }, 1000);

    // 清理函数：组件卸载时清除定时器
    return () => {
      clearInterval(timer);
    };
  }, []); // 空依赖，定时器只初始化一次

  return <div>已运行：{seconds}秒</div>;
}
```

如果不清理定时器，当组件被卸载（比如切换页面）后，定时器仍会继续执行，导致内存泄漏。清理函数确保组件卸载时定时器被正确清除。

条件性执行：依赖项变化时重新执行

```jsx
jsx 体验AI代码助手 代码解读复制代码function UserProfile({ userId }) {
  const [user, setUser] = useState(null);

  useEffect(() => {
    // 当userId变化时，重新请求用户信息
    const fetchUser = async () => {
      const response = await fetch(`https://api.example.com/user/${userId}`);
      const data = await response.json();
      setUser(data);
    };

    fetchUser();

    // 清理函数：取消未完成的请求（避免组件卸载后请求完成导致的问题）
    return () => {
      // 实际开发中可使用AbortController取消请求
    };
  }, [userId]); // 依赖userId，userId变化时重新执行

  if (!user) return <div>加载中...</div>;
  return <div>姓名：{user.name}</div>;
}
```

当`userId`变化（比如从 1 变成 2）时，副作用会重新执行，请求新的用户数据。这种写法确保 `UI` 能实时响应依赖项的变化。

