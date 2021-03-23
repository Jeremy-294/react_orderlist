# react_orderlist
# 使用React开发订单列表页及其评价模块

## 一、创建工程

>1.1 使用脚手架创建工程

```
npx create-react-app react-demo-orderlist
```

```
Success! Created react-demo-orderlist at F:\workspaces\sublime3\react-demo-orderlist\react-demo-orderlist
Inside that directory, you can run several commands:
mo
  npm start
    Starts the development server.

  npm run build
    Bundles the app into static files for production.

  npm test
    Starts the test runner.

  npm run eject
    Removes this tool and copies build dependencies, configuration files
    and scripts into the app directory. If you do this, you can’t go back!

We suggest that you begin by typing:

  cd react-demo-orderlist
  npm start

Happy hacking!
```

> 直接使用npm start来启动一下项目

***特别注意***

1. 设置ssh后还提示需要密码

> 记得不要少了(设置ssh需要在git bash中执行，windows的cmd环境不支持ssh-keygen命令)

```
ssh-add ~/.ssh/id_rsa 
ssh-add -l #仍然没有解决问题
```

## 二、规划目录及开发

### 2.1 操作步骤

```
创建src/components文件夹
创建src/components/Header文件夹
创建src/components/Header/index.js文件
创建src/components/Header/style.css文件
在index.js中输入rcc，快速创建出react基本代码结构并将组件名重命名为Header

继续创建App、OrderList、OrderItem文件夹目录
【可以把OrderItem放在OrderList文件夹下】
将App.css、App.js、App.test.js 都移动到App文件夹中
并将App.css、App.js重命名为style.css、index.js
修改启动src/index.js中的引用路径
并删除import * as serviceWorker from './serviceWorker';

根据浏览器中的报错提示修改页面中引用路径
```

### 2.2 OrderItem以及OrderList组件开发

使用BEM方式命名css

解析绑定字段

从父组件传递属性字段

字段判断绑定

OrderList中使用数组的map方法遍历

注意绑定key 以及代码注释语法

src/components/OrderList/index.js中引用OrderItem组件

src/components/App/index.js中引用OrderList组件

### 2.3 Header开发

src/components/App/index.js中引用Header组件

### 2.4 模拟ajax请求

json资源放在public文件夹下

在OrderList的componentDidMount () 钩子周期函数中调用api

```
componentDidMount (){
        fetch('/mock/orders.json').then(res => { //Promise对象
            // console.log(res) 
            if(res.ok){ // 包装对象中的值,不是api中的字段
                res.json().then(orderList=>{ 
                    this.setState(
                        {
                            orderList 
                        }
                    );
                })
            }

        })
    }
```

### 2.5 评价功能的样式

代码在OrderItem组件中

### 2.6 评价功能的点击

代码在OrderItem组件中

使用es6的箭头函数保证函数中的this执行当前组件的实例

注意子组件OrderItem向父组件OrderList传值

```
# 子组件OrderItem

<button className=' orderItem__btn--red'  onClick={this.handleSubmitComment}>提交</button>

 handleSubmitComment  = () => {
        const {id }            = this.props.data;
        const {comment,stars } = this.state;
        this.setState({
            editting: false
        })
        //提交服务器
        this.props.onSubmitCallback(id,comment,stars) // onSubmitCallback 在父组件中绑定在OrderItem组件上，应该在接口请求完成后执行
}

```

```
#父组件OrderList
  this.state.orderList.map(item => {  // testdata 
      return <OrderItem key={item.id} data={item} onSubmitCallback={this.handleSubmitCallback}/> 
  })

   handleSubmitCallback  = (id,comment,stars) => { //应该在接口请求完成后执行
      const newOrderList = this.state.orderList.map(item => {
            return item.id === id ? 
            {
                ...item,comment, stars, ifCommented: true  //es6语法，生成新对象
            }: item;
        });
      this.setState({
        orderList: newOrderList
      });
    }
```

## 三、编译打包发布

```
npm run build
```

> 注意文件生成在/build文件夹下

html中资源文件应该是从url的根目录下读取的，所以发布到gitpage上是404

编译完成后在index.html中手动修改url,或者在/static/ 前面加个.

/manifest 有一处，/static/有3处，记得保存

但是mock/orders.json的api前面加.好像无效，就手动写上吧。

