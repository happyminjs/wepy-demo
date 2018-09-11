
# WePY 使用记录
### 项目创建
```bash
    # 全局安装WePY
    npm install wepy-cli -g

    # 生成demo项目
    wepy init standard my-project

    # 切换到项目目录
    cd my-project

    # 安装依赖
    npm install

    # 开启实时编译
    wepy build --watch
```
### 目录结构
```bash 
    ├── dist                小程序运行代码目录（该目录由WePY的build指令自动编译生成，请不要直接修改该目录下的文件）
    ├── node_modules           
    ├── src                 代码编写的目录（该目录为使用WePY后的开发目录）
    |   ├── components      WePY组件目录（组件不属于完整页面，仅供完整页面或其他组件引用）
    |   |   ├── com_a.wpy   可复用的WePY组件a
    |   |   └── com_b.wpy   可复用的WePY组件b
    |   ├── pages           WePY页面目录（属于完整页面）
    |   |   ├── index.wpy   index页面（经build后，会在dist目录下的pages目录生成index.js、index.json、index.wxml和index.wxss文件）
    |   |   └── other.wpy   other页面（经build后，会在dist目录下的pages目录生成other.js、other.json、other.wxml和other.wxss文件）
    |   └── app.wpy         小程序配置项（全局数据、样式、声明钩子等；经build后，会在dist目录下生成app.js、app.json和app.wxss文件）
    └── package.json        项目的package配置
```
### 添加项目
微信开发者工具只是用来预览和调试，建议使用自己熟悉的IDE。  
>需要注意一点的在dist目录下的project.config.json文件中的几个配置要设置为false  
>es6：关闭ES6转ES5，否则运行会报错  
>postcss：关闭上传代码时样式自动补全，某些情况下漏掉此项运行会报错  
>minified：关闭代码压缩上传，否则会导致甄姬的computed、props.sync等属性失效  
### 项目规范
1、变量与方法名避免使用\$开头，因为WePY框架的内建属性和方法是以\$开头，在js中可用用this.的方式直接调用，[内建方法api文档](https://tencent.github.io/wepy/document.html#/api?id=api)。
2、小程序的入口、页面、组件文件必须是.wpy类型文件
3、使用ES6语法，使用Promise / async await
```bash
    # 在项目根目录，安装polyfill
    npm install wepy-async-function

    # 修改wepy.config.js，加入runtime配置
    babel: {
        "presets": [
            "env"
        ],
        "plugins": [
            "transform-export-extensions",
            "syntax-export-extensions"
        ]
    }

    # 在app.wpy中引入polyfill
    import 'wepy-async-function'

    # 在app.wpy中使用API Promise
    export default class extends wepy.app{
        constructor (){
            super();
            this.use('promisify');
        }
    }

    # 重启编译
    wepy build -no-cache
```
4、事件绑定替换  

    bindtap="click" => @tap="click",   
    catchtap="click" => @tap.stop="click";
    
    capture-bind:tap="click" => @tap.capture="click",  
    capture-catch:tap="click" => @tap.capture.stop="click";  
5、事件传参修改

    bindtap="click" data-index={{index}} => @tap="click({{index}})"








```bash

```