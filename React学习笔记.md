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

const styleObj = {
    color: num === 0? 'red' : 'green'
}
const handele = () => {
    
}
const Demo = function Demo(){
    
    let [num, setNum] = useState(0)
    
    return (
    	<div className= "demo">
            <span className="num" style={styleObj}>{num}</span>
            <Button type="primary" size="small" onClick={ handle }>新增</Button>
        </div>
    )
}
export default Demo
```



## 四、精品文章

### 1、`useEffect()`用法总结

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

