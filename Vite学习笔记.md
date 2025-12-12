# Vite

## 一、什么是构建工具

1、`typescript`：如果我们遇到`ts`文件我们需要使用`tsc`将`typescript`代码转换为`js`代码

2、`React/Vue`：安装`react-compiler/ vue-complier` ，将我们写的`jsx`文件或者`vue`文件转换为`render`函数

3、`less/sass/postcss/component-style`：我们有需要安装`less-loader`，`sass-loader`等一些列编译工具

4、语法降级：`babel` ---> 将`es`的新语法转换旧版浏览器可以接受的语法

5、体积优化：`uglifyjs`  ---> 将我们的代码进行压缩，变成体积更小性能更高的文件

6、...

稍微改一点点东西，非常麻烦



将`App.tsx`  => `tsc` => `App.jsx` => `React-complier` => `js`文件

有一个东西能够帮你把`tsc`，`react-complier`，`less`，`babel`， `uglifyjs`全部集成到一起，这个就叫做**构建工具**



打包：将我们写的浏览器不认识的代码，交给构建工具进行编译处理的过程就叫做打包，打包完成以后会给我们一个浏览器可以认识的文件



构建工具都做什么工作？

1、模块化开发支持：支持直接从`node_modules`里引入代码 + 多种模块化

2、处理代码兼容性：比如`babel`语法降级，`less`，`ts`语法转换（不是构建工具做的，构建工具将这些语法对应的处理工具集成进来，进行自动化处理）

3、提高项目性能：构建工具会自动监听文件的变化，当文件变化以后自动帮你调用对应的集成工具进行打包，然后在浏览器重新运行（整个过程叫做热更新，`hot replacement`）

​	开发服务器：跨域的问题，用`react-cli` `create-react-element` `vue-cli` 解决跨域问题



> [!TIP]
>
> 构建工具他让我们可以不用每次都关心我们的代码在浏览器如何运行，我们只需要首次给构建工具提供一个配置文件（这个配置文件也不是必须的，如果你不给他，他会默认的帮你去处理），有了这个集成的配置文件，我们就可以在下次需要更新的时候调用一次对应的命令就好了，如果我们再结合热更新，我们就更加不需要管任何东西，这就是构建工具去做的事情
>
> **他让我们不用关心生产的代码也不用关心代码如何在浏览器运行，只需要关系我们的开发怎么写的爽怎么写就好了**



市面上主流的构建工具有哪些？

> `webpack`、`vite`、`parcel`、`esbuild`、`rollup`、`grunt`、`gulp`

## 二、`vite`的优势

我们项目越大，构建工具索要处理的`js`代码就越多（跟`webpack`的一个构建过程（工作流程）有关系），构建工具需要很长时间才能启动开发服务器

`vite`会不会把`webpack`干翻，`vite`是基于`es modules`的，侧重点不一样，`webpack`更多关注兼容性，`vite`关注浏览器端的开发体验

## 三、`vite`脚手架和`vite`

比如我们输入```yarn create vite```

1、帮我们全局安装了一个东西：`create-vite`（`vite`的脚手架）

2、直接运行这个`create-vite` `bin`目录下的一个执行配置



我们之前接触过`vue-cli`

很多同学可能存在误区，**认为官网中使用对应`yarn create`构建项目过程也是`vite`在做的事情**



`create-vite`和`vite`的关系是什么呢？`create-vite`内置了`vite`，就像`vue-cli`会内置`webpack`一样

## 四、依赖预构建

**依赖于构建**首先`vite`会找到对应的依赖，然后调用`esbuild`（对`js`语法进行处理的一个库），将其他规范的代码转换为`esmodule`规范，然后放到当前目录下的`node_modules/.vite/deps`，同时对`esmodule`规范的各个模块进行统一集成

它解决了3个问题：

1、不同的第三方会有不同的导出格式（这个是`vite`没法约束人家的事情）

2、对路径的处理上可以直接使用`.vite/deps`，方便路径重写

3、叫做网络多包传输的性能问题（也是原生`esmodule`规范不敢支持`node_modules`的原因之一），有了依赖预构建以后，无论他有多少的额外`export`和`import`，`vite`都尽可能的将他们进行集成，最后只生成一个或者几个模块

## 五、`vite`开发环境

1、`vite`配置文件的语法提示

​	> 如果你使用的是`webstorm`，那你可以得到很好的语法补全

​	> 如果你使用是`vscode`或者其他编辑器，则需要做一些特殊处理



```js
//可以使用defineConfig函数进行语法补全
import { defineConfig } from 'vite'

export default defineConfig( config:{
    optimizeDeps:{
        exclude: [] //将指定数组中的依赖不进行依赖与预构建
    }
})
```

```js
//也可以使用@type注释方法进行语法补全

/* @type import("vite").UserConfig */
const viteConfig = {
    optimizeDeps:{
        exclude: [] //将指定数组中的依赖不进行依赖与预构建
    }
}

export defualt viteConfig
```

2、关于环境的处理

​	过去我们使用`webpack`的时候，我们要区分文件的一个环境

​	> `webpack.dev.config`

​	> `webpack.prod.config`

​	> `webpack.base.config`

​	> `webpackmerge`

```js
import { defineConfig } from 'vite'
import viteBaseConfig from './vite.base.config'
import viteDevConfig from './vite.Dev.config'
import viteProdConfig from './vite.Prod.config'

//策略模式
const envResolver = {
    "build": () => {
        console.log('生产环境')
        return ({...viteBaseConfig,...viteProdConfig})
	},
    "serve": {
        console.log('开发环境')
    	return Object.assign(target: {}, viteBaseConfig, viteDevConfig)
    }
}

export default defineConfig ( config: ({command: "build"|"serve"})=>{
	console.log('command', command)
    return envResolver[command]()
})

```

```js
// ./vite.base.config
import { defineConfig } from 'vite'

export default defineConfig( config:{
    optimizeDeps:{
        exclude: [] //将指定数组中的依赖不进行依赖与预构建
    }
})
```



## 六、`vite`环境变量

环境变量：会根据当前的代码环境产生值的变化的变量就叫做环境变量

代码变量：

​	1、开发环境

​	2、测试环境

​	3、预发布环境

​	4、灰度环境

​	5、生产环境



举个例子：百度地图`sdk`

`APP_KEY`: 测试环境和生产还有开发环境式不一样的`key`

- 开发环境：`110`
- 生产环境：`111`
- 测试环境：`112`



**服务端处理**

在`vite`中环境变量处理

`vite`内置了`dotenv`这个第三方库，会自动读取`.env`文件，并解析这个文件中的对应环境变量，并将其注入到`process`对象下，`process`的方法`env`方法可以查看（但是vite考虑到和其他配置的一些冲突问题，他不会直接注入到`procss`对象下，也就是说通过`console.log(process.env)`）拿不到环境配置的变换地址，`vite`给我们提供了一些补偿措施，我们可以调用`vite`的`loadEnv`来手动确认`env`文件

```
//涉及vite.config.js中的一些配置
- root
- envDir //用来配置当前环境变量的文件地址的

process.env方法，当前env文件所在的目录
process.cwd方法，返回当前node进程的工作目录
```

 

```js
import { defineConfig } from 'vite'
import viteBaseConfig from './vite.base.config'
import viteDevConfig from './vite.Dev.config'
import viteProdConfig from './vite.Prod.config'

//策略模式
const envResolver = {
    "build": () => {
        console.log('生产环境')
        return ({...viteBaseConfig,...viteProdConfig})
	},
    "serve": {
        console.log('开发环境')
    	return Object.assign(target: {}, viteBaseConfig, viteDevConfig)
    }
}

export default defineConfig ( config: ({command: "build"|"serve" , mode:string})=>{
	console.log('process', process.env)
    //当前env文件所在的目录，由于目前还没投return出结果，在这个地方调用env拿不到环境变量文件地址，此时我们可以使用process.cwd()方法
    const env = loadEnv(mode, process.cwd(), prefixes:"")//这里手动配置获取env环境文件，process.cwd返回的是当前工作目录，这里不一定要使用process.cwd()，也可以自己写上文件地址，字符串形式，第三个参数是文件的名字
    console.log("env",env)//此时就有了环境变量中的数据了
    return envResolver[command]()
})

```



> [!TIP]
>
> 补充一个小知识，为什么`vite.config.js`可以书写成`esmodule`的形式，这是因为`vite`他在读取这个`vite.config.js`时候会率先去解析文件语法，如果发现你是`esmodule`规范会直接将你的`esmodule`规范进行替换变成`commonjs`规范



`.env`：所有环境都需要用到的环境变量

​	> `.env.development`: 开发环境需要用到的环境变量（默认情况下`vite`将我们的开发环境取名为`development`）

​	> `.env.production`: 生产环境需要用到的环境变量（默认情况下`vite`将我们的开发环境取名为`production`）

`yarn dev --mode development`  这里的`--mode development`是启动`dev`环境的参数，并将`development`以字符串的形式传给`mode`，`mode`是`defineConfig`中的参数，此时`defineConfig`就能接收到`mode`为`development`，并赋值给`loadEnv`从而获取由其产生的环境变量的值

> [!TIP]
>
> 当我们调用`loadEnv`的时候，会做以下几个事情：
>
> 1、直接找到`.env`文件，并解析其中的环境变量，放进一个对象中
>
> 2、会将传来的`mode`这个变量进行拼接，如果`mode`是`development`，则`.env`后面拼接`development`，成为`.env.development`文件，并根据我们提供的目录去取对应的配置文件并进行解析，并放进一个对象
>
> 3、并将`.env.development`覆盖掉`.env`文件

```
// ./env.development
APP_kEY = 100
```



**客户端处理**（浏览器如果想获取变量值，参考下面）

如果是客户端，`vite`会将对应的环境变量注入到`import.meta.env`里面

`vite`做了一个拦截，他为了防止我们将隐私的变量直接送到`import.meta.env`中，所以他做了一层拦截，如果你的环境变量不是以`VITE`开头的，他就不会注入到客户端中去

```js
// ./env.development
APP_kEY = 100  //这样就拿不到APP_kEY的值   XXXXXX

VITE_APP_kEY = 100 //这样就能拿到APP_kEY的值了  √√√√√√
```

当然你也可以直接在配置变量中进行统一的配置增加或者改变前缀

```js
// ./vite.example.config.js
import { defineConfig } from 'vite'

export default defineConfig( config:{
    optimizeDeps:{
        exclude: [] //将指定数组中的依赖不进行依赖与预构建
    },
    envPrefix: "ENV_", //配置vite注入客户端环境变量校验的env前缀，也可以设置成其他的
})
```

经过以上调整后

```
// ./env.development
ENV_APP_kEY = 100  //这样也能通过vite的查验，并在客户端获取APP_KEY的值

```



配置package.json使用新变量开启测试

```
// ./package.json

//...
"script":{
	"div": "vite",
	"build": "vite build",
	"test": "vite --mode test
}
//..
```

此时命令行可以直接输入

```
yarn test
```

## 七、`vite`服务器的原理及文件解析

我们来实现一套简单的`vite`的开发服务器

1、探索为什么`vite`能认识`vue`后缀的文件

2、对开发服务器的底层原理有一个基础的认识

```js
// ./index.js

const Koa = require("koa")
const fs = require('fs')
const path = require('path')

//不同的宿主环境会给js赋予一些不同的能力

const app = new Koa()

//node最频繁的做的事情就是处理请求和操作文件

//当请求来临以后会直接进入use注册的或掉函数中
app.use(async (ctx) => {
    console.log('ctx', ctx.request, ctx.response)
    if (ctx.request.url === "/"){
        //这意味着其他人找我们要根路径的东西
        const indexContent = await fs.promises.readFile(path.resolve(__dirname, "./index.html")) //在服务端一般不会这么用
        ctx.response.body = indexContent
        ctx.response.set("Content-Type", "text/html")
    }
    if (ctx.request.url === '/main.js'){
        //将js结尾的内容展示出来
        //...
        ctx.response.set("Content-Type", "text/javascript")
    }
    if (ctx.request.url === '/app.vue'){
        //将将vue结尾的文件内容展示出来
        //...
    }
    if (ctx.request.url === '/api/getUserInfo'){
        //去数据库找到用户信息然后返回给前端
        //...
    }
})

app.listen(5173, ()=>{
    console.log('vite dev server listen on 5173')
})
```

> [!TIP]
>
> 总结：
>
> 1、通过`nodejs`的方法对本地文件进行查找，对查找到的以`vue`，`JS`，`CSS`等后缀的文件进行正则匹配，按照对应编译器进行字符串解析，将解析过的内容通过`nodejs`中的服务器框架搭建出来，并通过框架方法，返回到页面上

## 八、`vite`中对`css`的处理

`vite`天生就支持对`css`文件的直接处理

1、`vite`在读取到`JS`中引用的`CSS`

2、直接去使用`fs`模块去读取`CSS`中文件的内容

3、直接创建一个`style`标签，将`CSS`中的内容直接复制到style标签中

4、将`style`标签插入到`HTML`的`head`中

5、将`CSS`文件中的内容直接替换为`JS`较为（方便热更新和模块化），同时设置`Content-Type`为`JS`，从而让浏览器以`JS`脚本的形式执行该`CSS`后端文件



如果多个文件的`CSS`类名重复，就有可能导致样式被覆盖，这时，`cssmodule`就是来解决这个问题的

1、`module.css`（`module`是一种约定，表示需要开启`css`模块化）

2、他将你的所有类型名进行一定规则的替换

3、同时创建出一个映射对象

4、将替换后的内容塞进style标签里面放入head标签中

5、将`Component.module.css`内容进行全部清除，替换js脚本

6、将创建的映射对象在脚本中默认导出



- `modules`模块`css`的配置项

```js
// ./vite.example.config.js
import { defineConfig } from 'vite'

export default defineConfig( config:{
    optimizeDeps:{
        exclude: [] //将指定数组中的依赖不进行依赖与预构建
    },
    envPrefix: "ENV_", //配置vite注入客户端环境变量校验的env前缀，也可以设置成其他的
    css: { //对css的行为进行配置,
        modules: {
            //是对css模块化的默认行为进行覆盖,modules配置最终会丢给postcss_module
            localsConvention: "camelCase",//camelCase驼峰命名
            scopeBehaviour: "local",//local当前作用域模式
            gengerateScopedName: "[name]_[local]_[hash:5]",
            hashPrefix: 'hello'
            globalModulePaths: ["./component.module.css"]
        }
    }
})
```



`loaclsConvention`: 对样式类名的形式做出界定，

> `camelCase`驼峰命名，`camelCaseOnly`仅限驼峰命名，`dashes`中划线，`dashesonly`仅限中划线

`scopeBehaviour`:配置当前模块化行为是模块化还是全局化

> `local`本地模式（类名后接`hash`值确保类名唯一），`global`全局模式（类名就是类名本身）

`gengerateScopedName`: 配置本地作用域中类名后唯一值的命名方式

> 参考https://github.com/webpack/loader-utils#interpolatename
>
> ```
> gengerateScopedName:(name, filename, css) => {
> 	console.log(name, filename, css) //name此刻css文件的类名，filename此刻css的绝对路径，css此刻css的样式字符串
> 	return "xxx"
> }
> ```
>
> 配置成函数以后，返回值就决定了它最终显示的类型

`hashPrefix`:生成hash会根据你的类名去进行生成，参与hash的算法中，字符串形式赋给`hashprefix`

`globalModulePaths`：代表你不想参与到`css`模块化的路径，有些其他组件库的类名，你不想让他再根据`hash`再生成一个类名，可以用这个配置项，排除一些模组的`css`，这样可以保证你写的`css`与导入的`css`完全一样



- `preprocessorOptions`预处理器配置项

```js
// ./vite.example.config.js
import { defineConfig } from 'vite'

export default defineConfig( config:{
    optimizeDeps:{
        exclude: [] //将指定数组中的依赖不进行依赖与预构建
    },
    envPrefix: "ENV_", //配置vite注入客户端环境变量校验的env前缀，也可以设置成其他的
    css: { //对css的行为进行配置,
        modules: {
            //是对css模块化的默认行为进行覆盖,modules配置最终会丢给postcss_module
            localsConvention: "camelCase",//camelCase驼峰命名
            scopeBehaviour: "local",//local当前作用域模式
            gengerateScopedName: "[name]_[local]_[hash:5]",
            hashPrefix: 'hello'
            globalModulePaths: ["./component.module.css"]
        },
        preprocessorOptions: { // key:{config} key代表预处理器，config代表配置项
            less: {//整个的配置对象都会最终给到less的执行参数（全局参数）中去
                math: "always",
                globalVars: {
                    mainColor: 'red'//全局样式可以在这里配置，免去全局导入
                }
            },
            sass: {
                
            }
        }，
        devSourcemap:false//默认值为false
    }
})
```

假设没有构建工具，我们又想去编译less文件的话，等安装后可以使用`lessc`的编译器去编译less文件

- `devSourcemap`开启`css`的`sourcemap`的文件索引功能

`sourceMap`文件之间的索引：假设我们的代码被压缩或者被编译过了，这个时候假设程序出错，它将不会产生正确的错误位置

> true表示开启，false表示关闭，默认是false



## 九、`postcss`相关内容

`vite`天生对`postcss`有着非常良好的支持

功能：`postcss`保证`css`在执行的过程中是万无一失的，因为浏览器兼容问题，会触发各种各样的问题，`postcss`是为了保证各个浏览器均能良好使用`css`的工具

`babel`：帮助我们`js`执行起来万无一失，

```
:root {
	//css的新提案
	--globalColor: lightblue
}
```

`postcss`的操作如下

> 1、对未来`css`属性的一些使用降级问题
>
> 2、前缀补全：`google`非常卷 `--webkit`

我们写的`css`代码经过`postcss`处理将各种预处理器的语言如`sass`，`less`等编译成原生的`css`，然后前缀补全，最后输出到浏览器

> [!IMPORTANT]
>
> `postcss`误区：很多人都认为`postcss`与`less`、`sass`等是一类的预处理器，其实不是！postcss是后处理器，而less、sass是前处理器

```
1、安装依赖
yarn add postcss-cli postcss -D

2、书写描述文件
./postcss.config.js
//预设环境里面会包含很多的插件
const postcssPresentEnv = require("postcss-preset-env")

module.exports = {
	plugins: [postcssPresentEnv()]
}
```



```js
// ./vite.example.config.js
import { defineConfig } from 'vite'

import postcssPresentEnv from "postcss-preset-env"

export default defineConfig( config:{
    optimizeDeps:{
        exclude: [] //将指定数组中的依赖不进行依赖与预构建
    },
    envPrefix: "ENV_", //配置vite注入客户端环境变量校验的env前缀，也可以设置成其他的
    css: { //对css的行为进行配置,
        modules: {
            //是对css模块化的默认行为进行覆盖,modules配置最终会丢给postcss_module
            localsConvention: "camelCase",//camelCase驼峰命名
            scopeBehaviour: "local",//local当前作用域模式
            gengerateScopedName: "[name]_[local]_[hash:5]",
            hashPrefix: 'hello'
            globalModulePaths: ["./component.module.css"]
        },
        preprocessorOptions: { // key:{config} key代表预处理器，config代表配置项
            less: {//整个的配置对象都会最终给到less的执行参数（全局参数）中去
                math: "always",
                globalVars: {
                    mainColor: 'red'//全局样式可以在这里配置，免去全局导入
                }
            },
            sass: {
                
            }
        }，
        devSourcemap:true,//默认值为false
    },
    postcss: {
        plugins: [postcssPresentEnv()]
    }
})
```

## 十、路径path.resolve

`vite`一定会去读文件，我们如果写的是相对路径，那么`vite`会去尝试拼接成绝对路径，`commonjs` 规范 注入几个变量

`exports`, `require`, `module`,`__filename`,` __dirname`

```js
const fs = require("fs")
const path = require('path') //本质上就是字符串处理函数，里面很多路径处理方法

const result = fs.readFileSync('./variable.css')
const result = fs.readFileSync(path.resolve(__dirname, './variable.css'))

console.log("result", result.toString())
//node端去读取文件或者操作文件的时候，如果发现你用的是相对路径，则会去使用process.cwd()来进行拼接
//process.cwd获取当前执行环境的目录
```

## 十一、加载静态资源

什么是静态资源？

> 除了动态`API`以外，几乎都是静态资源
>
> 约定俗成的`assets`或者`public`文件夹中的东西
>
> `svg`: `scalable vector graphics` 可伸缩矢量图形（缺点是没法很好的展示层次丰富的图片信息），改变其颜色可以赋值给`style.fill`属性

```js
// ./src/imageloader

import sylasPicUrl from './assets/images/sylas.png?url'

//服务端 他会去读取这个图片文件的内容----> Buffer 二进制字符串

const img = document.createElement('img')

img.src = sylasPicUrl

document.body.append(img)
```

```js
// ./main.js
import "./src/imageloader"
import jsonFile from './src/assets/json/index.json'
import { name } from './src/assets/json/index.json'

//lodash：js工具库，深度克隆
import _ from "lodash" //XXXX,因为这样会全部导入，
import { cloneDeep } from "lodash"  //√√√√，因为这样只会导入需要的库，不需要的不导入，摇树优化，提高代码性能

_.deepClone

//企业项目：如果生产环境非常臃肿和性能差
//控制导入

//tree shaking 摇树优化：打包工具会自动帮你移除掉你没有用到的变量和方法
console.log("jsonFile",jsonFile)//如果你用的不是vite，在其他的一些构建工具中，json文件的导入会作为一个json字符串形式存在
```

```json
// ./src/assets/json/index.json
{
    "name": "alice",
    "age": "18"
}
```

> [!TIP]
>
> 1、按需导入，避免臃肿
>
> 2、地址添加标识，导入位可加`raw`、`url`等
>
> 3、`vite`不能默认转换`json`字符串，导入仍然是对象



如果导入级别过多，太难看，可以进行@别名导入

```js
// ./vite.example.config.js
import { defineConfig } from 'vite'
import path from 'path'

import postcssPresentEnv from "postcss-preset-env"

export default defineConfig( config:{
    resolve: {
        alias: {
			"@":path.resolve(__dirname, "./src"),
    		"@assets": path.resolve(__dirname, "./src/assets")
        }                      
    },
    optimizeDeps:{
        exclude: [] //将指定数组中的依赖不进行依赖与预构建
    },
    envPrefix: "ENV_", //配置vite注入客户端环境变量校验的env前缀，也可以设置成其他的
    css: { //对css的行为进行配置,
        modules: {
            //是对css模块化的默认行为进行覆盖,modules配置最终会丢给postcss_module
            localsConvention: "camelCase",//camelCase驼峰命名
            scopeBehaviour: "local",//local当前作用域模式
            gengerateScopedName: "[name]_[local]_[hash:5]",
            hashPrefix: 'hello'
            globalModulePaths: ["./component.module.css"]
        },
        preprocessorOptions: { // key:{config} key代表预处理器，config代表配置项
            less: {//整个的配置对象都会最终给到less的执行参数（全局参数）中去
                math: "always",
                globalVars: {
                    mainColor: 'red'//全局样式可以在这里配置，免去全局导入
                }
            },
            sass: {
                
            }
        }，
        devSourcemap:true,//默认值为false
    },
    postcss: {
        plugins: [postcssPresentEnv()]
    }
})
```

以上导入就变为

```
// @assets/json/index.json
{
    "name": "alice",
    "age": "18"
}
```

## 十二、生产环境的相关配置

当我们将工程进行打包以后，会发现找不到原来的资源，这是因为disk文件夹不在跟目录下导致的，请将`disk`文件夹转移至设为运行环境目录即可（用`disk`文件夹打开终端）

打包后静态资源为什么要有`hash`？浏览器有一个缓存机制，静态资源名字只要不改，那么他就会直接用缓存的，下一次刷新页面，请求的名字如果是同一个，就会读取缓存，所以我们要避免名字一致（文件更改了，文件名字却没有变化，从而更新的文件没有搞到disk里面，导致打包出错）

`hash`算法：将一串字符串经过运算得到一个新的乱码字符串

```js
// ./vite.example.config.js
import { defineConfig } from 'vite'
import path from 'path'

import postcssPresentEnv from "postcss-preset-env"

export default defineConfig( config:{
    resolve: {
        alias: {
			"@":path.resolve(__dirname, "./src"),
    		"@assets": path.resolve(__dirname, "./src/assets")
        }                      
    },
    optimizeDeps:{
        exclude: [] //将指定数组中的依赖不进行依赖与预构建
    },
    envPrefix: "ENV_", //配置vite注入客户端环境变量校验的env前缀，也可以设置成其他的
    css: { //对css的行为进行配置,
        modules: {
            //是对css模块化的默认行为进行覆盖,modules配置最终会丢给postcss_module
            localsConvention: "camelCase",//camelCase驼峰命名
            scopeBehaviour: "local",//local当前作用域模式
            gengerateScopedName: "[name]_[local]_[hash:5]",
            hashPrefix: 'hello'
            globalModulePaths: ["./component.module.css"]
        },
        preprocessorOptions: { // key:{config} key代表预处理器，config代表配置项
            less: {//整个的配置对象都会最终给到less的执行参数（全局参数）中去
                math: "always",
                globalVars: {
                    mainColor: 'red'//全局样式可以在这里配置，免去全局导入
                }
            },
            sass: {
                
            }
        }，
        devSourcemap:true,//默认值为false
    },
    postcss: {
        plugins: [postcssPresentEnv()]
    }，
    build: {
        rollupOption:{//配置rollup的一些构建策略
            output: {//控制输出
                assetFileName:"[hash].[name].[ext]"
            }
        },
        assetsInlineLimit: 4096, //小于4kb的转化为base64字符，大于4kb的才会转换为静态资源文件
        outDir: "dist",//配置输出目录名称
        assetsDir: "static",//配置静态资源目录名称
        emptyOutDir: true, //默认就是true，所以不用设置，清除输出目录所有文件
    }
})
```

`rollupOption-output-assetFileName`: 配置`rollup`的一些配置项

> `ext` 表示输出的文件格式，如果是`css`文件，那么导出的也是`css`文件
>
> `name` 表示文件的名字
>
> `hash` 表示将你的文件名和文件内容进行组合计算得来的结果

`assetsInlineLimit` : 限制资源大小

> 设置一个数字类型的数，单位为`kb`，如果小于`xxkb`的，将转化为`base64`字符，大于`xxkb`的才会转换为静态资源文件

`outDir`:设置输出目录的名字

> 设置一个字符串，赋值给`outDir`

`assetsDir`:设置静态资源目录的名字

> 设置一个衣服穿，赋值给`assetsDir`

## 十三、`vite`插件

`vite`会在生命周期的不同阶段中去调用不同的插件以达到不同的目的

- `vite-aliases`

可以帮我们自动生成别名，检测你当下目录下包括`src`在内的所有文件夹，并帮助我们去生成别名

```js
// ./vite.example.config.js
import { defineConfig } from 'vite'
import path from 'path'
import { ViteAliases } from 'vite-aliases' //导入插件

import postcssPresentEnv from "postcss-preset-env"

export default defineConfig( config:{
    resolve: {
        alias: {
			"@":path.resolve(__dirname, "./src"),
    		"@assets": path.resolve(__dirname, "./src/assets")
        }                      
    },
    optimizeDeps:{
        exclude: [] //将指定数组中的依赖不进行依赖与预构建
    },
    envPrefix: "ENV_", //配置vite注入客户端环境变量校验的env前缀，也可以设置成其他的
    css: { //对css的行为进行配置,
        modules: {
            //是对css模块化的默认行为进行覆盖,modules配置最终会丢给postcss_module
            localsConvention: "camelCase",//camelCase驼峰命名
            scopeBehaviour: "local",//local当前作用域模式
            gengerateScopedName: "[name]_[local]_[hash:5]",
            hashPrefix: 'hello'
            globalModulePaths: ["./component.module.css"]
        },
        preprocessorOptions: { // key:{config} key代表预处理器，config代表配置项
            less: {//整个的配置对象都会最终给到less的执行参数（全局参数）中去
                math: "always",
                globalVars: {
                    mainColor: 'red'//全局样式可以在这里配置，免去全局导入
                }
            },
            sass: {
                
            }
        }，
        devSourcemap:true,//默认值为false
    },
    postcss: {
        plugins: [postcssPresentEnv()]
    }，
    build: {
        rollupOption:{//配置rollup的一些构建策略
            output: {//控制输出
                assetFileName:"[hash].[name].[ext]"
            }
        },
        assetsInlineLimit: 4096, //小于4kb的转化为base64字符，大于4kb的才会转换为静态资源文件
        outDir: "dist",//配置输出目录名称
        assetsDir: "static",//配置静态资源目录名称
        emptyOutDir: true, //默认就是true，所以不用设置，清除输出目录所有文件
    },
    plugins: [
        ViteAliases()
    ],
})
```

```js
//...
plugins: [
	ViteAliases({
		prefix: "&"//将@改为&
	})
]
//...
```



```js
|-src
	|-assets
	|-components
	|-pages
	|-store
	|-utils
	
[
	{
		find:'@',
		replacement: `${your_project_path}/src`
	},
	{
		find:'@assets',
		replacement: `${your_project_path}/src/assets`
	},
	{
		find:'@components',
		replacement: `${your_project_path}/src/components`
	},
	{
		find:'@pages',
		replacement: `${your_project_path}/src/pages`
	},
	{
		find:'@store',
		replacement: `${your_project_path}/src/store`
	},
	{
		find:'@utils',
		replacement: `${your_project_path}/src/utils`
	}
]
```



以下是手写`vite-aliases`的代码

这里使用了`config`周期函数，他是在配置文件生效前做出的变更钩子

```js
// vite的插件必须返回vite一个配置对象
const fs = require('fs')
const path = require('path')

function diffDirAndFile(dirFileArr = [], basePath=''){
    const result = {
        dirs: [],
        files: []
    }
    dirFileArr.forEach(name => {
        const currentFileStat = fs.statSync(path.resolve(__dirname, basePath + "/" + name))
        const isDirectory = currentFileStat.isDirectory()
        
        if(isDirectory){
            result.dirs.push(name)
        }else{
            result.files.push(name)
        }
    })
}

function getTotalSrcDir(){
    const diffResult = diffDirAndFile(result, '../src')
    const resolveAliasesObj = {}  //放的就是一个一个的别名配置
    diffResult.dirs.forEach(dirname => {
        const key = `@${dirname}`
        const absPath = path.resolve(__dirname, '../src'+'/'+dirname)
        resolveAliasesObj[key] = asbPath
    })
    return resolveAliasesObj
}

module.exports = () => {
    return{
        config(config, env){
            //config函数可以返回一个对象，这个对象是部分的viteconfig配置
            //production development serve build
            //env:  mode:string  command: string 
            const resolveAliasesObj = getTotalSrcDir()
            return {
                resolve: {
                    alias: resolveAliasesObj
                }
            }
        }
    }
}
```



- `vite-plugin-html`

生成`html`的插件

```js
// ./vite.example.config.js
import { defineConfig } from 'vite'
import path from 'path'
import { ViteAliases } from 'vite-aliases' //导入插件
import { createHtmlPlugin } from 'vite-plugin-html'

import postcssPresentEnv from "postcss-preset-env"

export default defineConfig( config:{
    resolve: {
        alias: {
			"@":path.resolve(__dirname, "./src"),
    		"@assets": path.resolve(__dirname, "./src/assets")
        }                      
    },
    optimizeDeps:{
        exclude: [] //将指定数组中的依赖不进行依赖与预构建
    },
    envPrefix: "ENV_", //配置vite注入客户端环境变量校验的env前缀，也可以设置成其他的
    css: { //对css的行为进行配置,
        modules: {
            //是对css模块化的默认行为进行覆盖,modules配置最终会丢给postcss_module
            localsConvention: "camelCase",//camelCase驼峰命名
            scopeBehaviour: "local",//local当前作用域模式
            gengerateScopedName: "[name]_[local]_[hash:5]",
            hashPrefix: 'hello'
            globalModulePaths: ["./component.module.css"]
        },
        preprocessorOptions: { // key:{config} key代表预处理器，config代表配置项
            less: {//整个的配置对象都会最终给到less的执行参数（全局参数）中去
                math: "always",
                globalVars: {
                    mainColor: 'red'//全局样式可以在这里配置，免去全局导入
                }
            },
            sass: {
                
            }
        }，
        devSourcemap:true,//默认值为false
    },
    postcss: {
        plugins: [postcssPresentEnv()]
    }，
    build: {
        rollupOption:{//配置rollup的一些构建策略
            output: {//控制输出
                assetFileName:"[hash].[name].[ext]"
            }
        },
        assetsInlineLimit: 4096, //小于4kb的转化为base64字符，大于4kb的才会转换为静态资源文件
        outDir: "dist",//配置输出目录名称
        assetsDir: "static",//配置静态资源目录名称
        emptyOutDir: true, //默认就是true，所以不用设置，清除输出目录所有文件
    },
    plugins: [
        ViteAliases()//别名插件
        createHtmlPlugin({ //ejs模板插件
        	inject: {
        		data:{
        			title: "主页"
        		}
        	}
        })
    ],
})
```



以下手写这个插件`vite-plugin-html`

这里使用了`tarnsformIndexHtml`钩子函数

```js
// ./createHtmlPlugin.js

module.exports={
	return{
		tarnsformIndexHtml:{
    		enforce: "pre",//可以在vite核心插件之前调用该插件，下有官方说明
    		transform:(html, ctx) => {
            //ctx 表示当前整个请求的一个执行期上下文
                return html.replace(/<%= title %>/g, option.inject.data.title)
        	}
		}
	}
}
```

**强制插件排序**

为了与某些 `Rollup` 插件兼容，可能需要强制修改插件的执行顺序，或者只在构建时使用。这应该是 `Vite` 插件的实现细节。可以使用 `enforce` 修饰符来强制插件的位置:

> `pre`：在 `Vite` 核心插件之前调用该插件

> 默认：在 `Vite` 核心插件之后调用该插件

> `post`：在 `Vite` 构建插件之后调用该插件



- `vite-plugin-mock`

`mock`数据：模拟数据

前后端一般是并行开发 用户列表（ 接口文档 ）

1、简单方式：直接去写一两个数据，方便调试，缺点是没办法做海量测试，没办法获取一些标准数据，没办法去感知`http`的异常

2、使用`mockjs`：模拟海量数据

```js
// ./vite.example.config.js
import { defineConfig } from 'vite'
import path from 'path'
import { ViteAliases } from 'vite-aliases' //导入插件
import { createHtmlPlugin } from 'vite-plugin-html'//导入插件
import { viteMockServe } from 'vite-plugin-mock'//导入插件

import postcssPresentEnv from "postcss-preset-env"

export default defineConfig( config:{
    resolve: {
        alias: {
			"@":path.resolve(__dirname, "./src"),
    		"@assets": path.resolve(__dirname, "./src/assets")
        }                      
    },
    optimizeDeps:{
        exclude: [] //将指定数组中的依赖不进行依赖与预构建
    },
    envPrefix: "ENV_", //配置vite注入客户端环境变量校验的env前缀，也可以设置成其他的
    css: { //对css的行为进行配置,
        modules: {
            //是对css模块化的默认行为进行覆盖,modules配置最终会丢给postcss_module
            localsConvention: "camelCase",//camelCase驼峰命名
            scopeBehaviour: "local",//local当前作用域模式
            gengerateScopedName: "[name]_[local]_[hash:5]",
            hashPrefix: 'hello'
            globalModulePaths: ["./component.module.css"]
        },
        preprocessorOptions: { // key:{config} key代表预处理器，config代表配置项
            less: {//整个的配置对象都会最终给到less的执行参数（全局参数）中去
                math: "always",
                globalVars: {
                    mainColor: 'red'//全局样式可以在这里配置，免去全局导入
                }
            },
            sass: {
                
            }
        }，
        devSourcemap:true,//默认值为false
    },
    postcss: {
        plugins: [postcssPresentEnv()]
    }，
    build: {
        rollupOption:{//配置rollup的一些构建策略
            output: {//控制输出
                assetFileName:"[hash].[name].[ext]"
            }
        },
        assetsInlineLimit: 4096, //小于4kb的转化为base64字符，大于4kb的才会转换为静态资源文件
        outDir: "dist",//配置输出目录名称
        assetsDir: "static",//配置静态资源目录名称
        emptyOutDir: true, //默认就是true，所以不用设置，清除输出目录所有文件
    },
    plugins: [
        ViteAliases(),//别名插件
        createHtmlPlugin({ //ejs模板插件
        	inject: {
        		data:{
        			title: "主页"
        		}
        	}
        }),
        viteMockServe()
    ],
})
```

根目录下新建`mock`文件夹，新建`index.js`，这个是`viteMockServe()`插件自动扫描的目录

```js
// ./mock/index.js

import mockJS from 'mockjs'

const userList = mockJS.mock({
    "data|100":[{
        name: "@name",
        "id|+1": 1,
        time: "@time",
        date: "@data"
    }]
})

module.exports = [
    {
        method: "post",
        url: "/api/users",
        response: ({body}) => {
            return{
                code:200,
                msg: "success",
                data: userlist
            }
        }
    }
]
```



手写`vite-plugin-mock`

这里使用了`configureServer`钩子函数，该函数主要用于配置网络服务

```js

export default (option) => {
	//做的最主要的就是拦截http请求
	return{
		configureServer(server) {
            //req,请求对象
            //res,相应对象
            //next,移交给下一个中间件
            server.middlewares.use((req, res, next) => {
            	if (req.url === "/api/user") {
                    res.end('hello world')
                    //判断mock文件夹中index的方法中的url是否存在，如果url存在，则将地址合并成一个绝对地址，并返回对应数据
                }else{
                    next()
                }  
            })
        }
	}
}
```

> [!TIP]
>
> 总结
>
> --------纯`vite`插件------------------
>
> - `config` 配置钩子
> - `configureServer` 服务器钩子
> - `transformIndexHtml` 文档钩子
>
> 
>
> - `configResolved `
>   - 整个配置文件的解析流程完全完毕以后才会走的钩子
>   - 可以查看最后的配置文件
> - `configurePreviewServer `
>   - 可以预览开发版本的生产版本效果
> - `handleHotUpdate`
>   - 热更新功能
>
> ------------`vite`及`rollup`都兼容的插件------------------
>
> - `options`
>
>   - ```
>     options(rollupOptions){
>     	//主要是使用rollup打包的相关配置项
>     }
>     ```
>
>   ...

## 十四、`vite`与`TS`语言结合

`ts`是`js`的一个类型检查工具，检查我们代码中可能会存在的一些隐形问题，同时给我们一些语法提示

安装`vite-plugin-checker`：该插件主要用于检查`TS`是否出错

```ts
// ./vite.config.ts

import { defineConfig } from "vite"
import checker from "vite-plugin-checker"

export default defineConfig({
	plugins: [checker({
		typescript: true
	})]
})
```

```json
// ./tsconfig.json

{
    "compilerOptions":{
        "skipLibCheck": true, //是否跳过node_module目录的检查,该目录下都是生产的包，已经验证过了，一般不需要检查，设置成true，可以跳过
        "module":"ESNext",//默认是es3，会导致很多方法失效，如果出现错误，尝试修改这里
    }
}
```

```json
// ./package.json

//...
"script":{
	"dev": "vite",
	"build": "tsc --noEmit && vite build"//如果ts编译错误，将不进行build打包
}
//...
```

```ts
// ./vite-env.d.ts
//以下是三斜线指令，
///<reference types="vite/client" />

interface importMetaEnv {
    readonly VITR_PROXY_TARGER: string
}
```

```
// .env
VITR_PROXY_TARGER=www.baidu.com
```

```
// .main.ts
console.log("meta", import.meta.env.VITR_PROXY_TARGER)
```



## 十五、前端性能优化

我们平时说的性能优化是说什么东西？

- 开发时态的构建速度优化（yarn dev/yarn start敲下的一瞬间呈现结果要占用多长时间）

  - webpack在这方面下的功夫比较重，比如webpack的cache-loader cache loader结果（如果两次构建的源代码没有变化，则直接使用缓存，不用调用loader），thread-loader，开启多线程去构建...
  - vite按需加载，所以我们不太在乎构建速度

- 页面渲染指标：和我们怎么去写代码有关

  - 首屏渲染时：fcp（first content paint）

    - 懒加载：需要我们去写代码实现

    - http优化：协商缓存 强缓存

      > 强缓存：服务端给响应头追加一些字段（expires），客户端会记住这些字段，在expires截至失效前，无论你怎么刷新页面，浏览器都不会重新请求页面，而是从缓存里取
      >
      > 协商缓存： 是否使用缓存要跟后端商量一下，当服务端给我们打上协商缓存的标记以后，客户端在下次刷新页面需要重新请求资源时会发送一个协商请求给到服务器，服务端如果说需要变化，则会响应具体的内容，如果服务端觉得没变化则会响应304

  - 页面中最大元素的一个时常：lcp（largest content paint）

  - ...

- js逻辑

  - 我们要注意副作用的清除，组件会频繁的挂载和卸载，如果我们在某一个组件中有计时器，如果我们再卸载的时候不去清除这个计时器，下次再次挂载的时候等于开了两个线程
  - 我们在写法上一个注意事项：requestAnimationFrame， 卡浏览器帧率 对浏览器渲染原理要有一定的认识，然后再这方面在做优化
    - requestIdleCallback：传一个函数进去
    - 浏览器的帧率：16.6ms去更新一次（执行js逻辑，以及 重排重绘），假设我们js执行逻辑超过16.6，掉帧了
    - 为此，react有一些方案concurrent mode 或者 concurrency，可终端渲染
  - 防抖-节流， lodash js 工具，上面的方法更成熟一些
  - 对作用域的一个控制

- 构建优化（vite（rollup） webpack）

  - 优化体积：压缩、treeshaking、 图片资源压缩、 cdn加载、 分包

### （1）分包策略

浏览器的缓存策略

静态资源 ---> 名字没有变化，那么他就不会重新去拿 xxx.js

hash：只要内容有一点的变化，hash字符串就不一样

分包：就是把一些不会常规更新的文件，进行单独打包

​	安装lodash，安装@type/lodash

```ts
// ./main.ts

import { forEach } from "lodash"

const mainArr = []

forEach(mainArr, (elm) => {
    console.log(elm)
})
```

```json
// ./tsconfig.json

{
    "compilerOptions":{
    	"moduleResolution": "node",//安装的lodash后，根据本路径寻找包，找不到以后会向上级目录找，此项配置后，可以直接去node_module环境中查找lodash
        "skipLibCheck": true, //是否跳过node_module目录的检查,该目录下都是生产的包，已经验证过了，一般不需要检查，设置成true，可以跳过
        "module":"ESNext",//默认是es3，会导致很多方法失效，如果出现错误，尝试修改这里
        "lib": ["ES2017","DOM"]
    }
}
```

```js
// ./vite.example.config.js
import { defineConfig } from 'vite'

export default defineConfig( {
    "build": {
      	"minify": false, //构建乱码，关掉。后期再查
        "rollupOption":{
            "input": {
                main:path.resolve(__dirname, "./index.html"),
                product:path.resolve(__dirname, "./product.html")
            },
            "output":{
                "manualChunks":(id) => {
                    if (id.includes("node_modules")){
                        return "vendor"
                    }
                }
            }
        }
    },
	//...
})
```

### （2）gzip压缩

有时候，我们的文件资源实在太大了，比如一个`js`文件有`2mb`，`http`传输

办法：将所有的静态文件进行压缩，以达到减少体积的目的

服务端压缩文件

客户端收到压缩包进行解压缩

从入口文件到它一些列依赖最终打包成`js`文件叫做块，块最终会映射成`js`文件，但是块不是`js`文件



安装`viteCompression`

```
// ./vite.config.ts

import { defineConfig } from "vite"
import checker from "vite-plugin-checker"

export default defineConfig({
	plugins: [
		viteCompression()
	]
})
```



### （3）动态导入

vite是按需加载的，动态导入是es6的一个新特性，

```
import().then()
```

### （4）cdn加速

cdn：content delivery network

将我们依赖的第三方模块写成cdn的形式，然后保证我们自己代码的一个小体积

安装插件vite-plugin-cdn-import

```
// ./vite.config.ts

import { defineConfig } from "vite"
import viteCDNPlygin from "vite-plugin-cdn-import"

export default defineConfig({
	plugins: [
		viteCDNPlygi({
			modules:{[
				name:"lodash",
				var: "_",
				path: "https://cdn.jsdelivr.net/npm/lodash@4.17.21/lodash.min.js"
			]}
		})
	]
})
```



## 十六、跨域原理解读

同源策略：仅在浏览器发生，浏览器的规则

http交互默认情况下只能在同协议同域名同端口的两台终端进行



跨域：当A源 向 B源 （不满足同源策略，满足同源限制），请求对应信息，就会产生跨域，跨域请求默认情况下会被浏览器拦截，除非对应的请求服务器出具凭证才会放行数据传递

```
// ./vite.config.ts

import { defineConfig } from "vite"
import viteCDNPlygin from "vite-plugin-cdn-import"

export default defineConfig({
	server: {
		proxy: {
            "api":{
                target: "http://xxxx",
                changeOrigin:true,
                rewrite: (path) => path.replace(/^\/api/, ''/)

            }
		}
	}
})
```

