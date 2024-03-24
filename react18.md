

##  1.React技术栈构成

```
脚手架工具创建不同的react项目 ：create-vite  create-react-app Next.js  Remin

路由:  react-router-dom

状态管理: redux mobx Zustand

ui组件库: Ant-Design  Material-ui

hooks 库: ahooks  react-use
```



## 2.React 脚手架创建项目

+ 官方脚手架

```
npx create-react-app my-app
cd my-app
npm start
```

+  vite 

```
npx create-vite@latest 
JavaScript + SWC    
##Swc 号称比bablejs 打包快70倍
## SWC是基于Rust语言的JavaScript Compiler(其生态中也包含打包工具spack), 目前为Next.JS/Parcel/Deno等前端圈知名项目使用.
   swc与babel还是有一定的差距，如果有兼容性要求很严格的项目还是不要轻易尝试了，但是如果只需要兼容最近的主流浏览器也是基本没问题的。
```

![image-20231119113854522](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231119113854522.png)

+ strictModel 严格模式

  > 用于检查组件是否是纯函数
  >
  > 及早发现useEffect中的错误
  >
  > 警告过时的API  

## 3.配置 ESlint 管理代码格式

```
yarn add vite-plugin-eslint
```



## 4.react 和 react-dom 模块的区别

+ react 

  核心功能，全局组件都在这

  

+  react-dom  用于操作浏览器dom

   react-dom/client    客户端                  react-dom/server (用于服务端渲染)

​	把react  和 react-dom 拆开涉及，是为了解耦， react-native 开发就没用引用react-dom这个模块

## 5.父子组件props 传值的写法

```
props   整体接收
//子组件
function Welcome(props) {
  return (
    <div>
      {props.name} : {props.age}
    </div>
  )
}
//子组件  解构props 接收
function Welcome({name,age}) {
  return (
    <div>
      {name} : {age}
    </div>
  )
}


//父组件
function App() {
  return (
    <>
      <div>hello word</div>
      // name="张三" age="22"  传入的是固定值   name = { }  age = { }  动态值需要用 { }
      <Welcome name="张三" age="22"></Welcome>
    </>
  )
}

假如 父组件传递的属性比较多 一个个的写太麻烦   {...dataInfor}

//子组件
function Welcome({name,age,timer}) {  //需要什么自己解构什么
  return (
    <div>
      {name} : {age} : {timer}
    </div>
  )
}
//父组件
function App() {
  let dataInfor ={
    name:"z-xcv",
    timer:"2022-10-11",
    workder:"web",
    age:"22",
    likes:["读书","运动"]
  }
  return (
    <>
      <div>hello word</div>
      <Welcome {...dataInfor}></Welcome>
    </>
  )
}
```

##  6.组件是如何组合在一起

```
#props.children 这个react 自定义的属性用于渲染 子组件

function MyBottom(){
  return(
    <div>my bottom</div>
  )
}
//子组件
function Welcome({ children }) {
  return (
    <div>
      <div> body</div>
       { children } 			##渲染子组件
    </div>
  )
}
//父组件
function App() { 
  return (
    <>
      <div>header</div>
      <Welcome>       # 在Welcome 组件中插入了 MyBottom组件   Welcome 就必须解构 { children } 
        <MyBottom/>
      </Welcome>
    </>
  )
}

//上面的 children 只能一次渲染，并且不能改变里面的顺序

function Welcome({top, bottom}) {
  return (
    <div>
      {top}
      <div> body</div>
      {bottom}
    </div>
  )
}
//父组件
function App() { 
  return (
    <>
      <div>header</div>
      <Welcome 
      top={<div>top div</div> }				# { } 中不仅能传递属性，函数，还能传递vdom 组件
      botom={<div>bottom div</div> }
      >
      </Welcome>
    </>
  )
}

function Tst(){
  return (
    <div>top bottom</div>
  )
}

function Welcome({top, bottom}) {
  return (
    <div>
      {top}
      <div> body</div>
      {bottom}
    </div>
  )
}
//父组件
function App() { 
  return (
    <>
      <div>header</div>
      <Welcome 
      top={ <Tst></Tst> }      # 传递 vdom 函数组件
      botom={<div>bottom div</div> }
      >
      </Welcome>
    </>
  )
}
页面渲染跟之前一样
```

![image-20231119163336940](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231119163336940.png)

##  7.组件传值设置默认值的方式

```
方式1   es6 的默认参数赋值 可以做到 牢记函数组件，其实就跟写js函数一样， 
		js很灵活，写react 不是写html，更多的是写js。

function Welcome({name="zhangsan", age="14"}) {
  return (
    <div>
     {`${name} : ${age}`}
    </div>
  )
}
//父组件
function App() { 
  return (
    <>
      <div>header</div>
      <Welcome></Welcome>
    </>
  )
}

方式2   组件名.defaultProps = { }
function Welcome({name, age}) {
  return (
    <div>
     {`${name} : ${age}`}
    </div>
  )
}
Welcome.defaultProps={
  name:"张韶涵",
  age: 18
}

```

##  8.组件指定props 类型限制

```
import propTypes from "prop-types";   //vite 脚手架已经集成了这个插件  引入使用
function Welcome({ name, age }) {
  return <div>{`${name} : ${age}`}</div>
}
Welcome.defaultProps = {  // 设置默认值
  name: '张韶涵',
  age: 18,
}
Welcome.propTypes = {		//限制props传参 的类型
  name: propTypes.string,
  age:  propTypes.number,
  nikeName: propTypes.oneOfType(  //多类型满足一个就验证通过
      name: propTypes.string,
      age:  propTypes.number,
  ),
  type: propTypes.oneOf(['primary','success','error']),   // 只能是其中一个固定值，不能传递数组范围之外的值
  icon : propTypes.element   //jsx 类型
}
//父组件
function App() {
  return (
    <>
      <div>header</div>
      <Welcome
      name={ "张韶涵" } 
      age={ 18 }
      type="primary"
      icon = { <div> <i class="iconfont icon-close"> </i> </div>}
      ></Welcome>
    </>
  )
}

```

## 9. 组件必须是一个纯函数

```
1.纯函数的定义
  只负责自己的任务，调用时候不会更改 之前就存在的变量或者对象
  给定相同的输入， 输出必定相同。
  
  let str="hello"    
  function changeStr(){  //调用的时候修改了外部作用域str  不是纯函数
   str="hello111"
  }
  changeStr();    
  
 // <React.StrictMode> 会调用2次 App()函数  所以打印3 , 严格模式检查组件是否是一个纯函数
 //这么写不会报错，但是不符合规范
let num  =1;
function App() {
  num++;
  console.log(num);
  return (
    <>
      <div>header</div>
     
    </>
  )
}

```

![image-20231119171548179](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231119171548179.png)

## 10. 组件的状态和useState

+ 根据用户不同的操作而改变的数据，称为组件的state， 数据（状态）是可以驱动试图改变的 ，普通变量不行。 

```
## 格式 let [变量名称，操作变量的函数] = useState(初始值)

import { useState } from "react"
function App() {
  let [count, setCount] = useState(0)  
  
  const handlClick = ()=>{
     setCount(count+1)
  }
  return (
    <>
      <div>header</div>
      <button onClick={handlClick}>+</button>
      {count}
    </>
  )
}

export default App

```

## 11.useState快照和快照陷阱

```
以下代码打印count 的值是多少

import { useState } from "react"
function App() {
  let [count, setCount] = useState(0)

  const handlClick = ()=>{
     setCount(count+1)
     setCount(count+1)
     setCount(count+1)
     console.log(count);
     setTimeout(()=>{
     console.log(count);
     },2000)
  }
  return (
    <>
      <div>header</div>
      <button onClick={handlClick}>+</button>
      {count}
    </>
  )
}

理解: JS 闭包的作用，和函数作用域 

第一次渲染 页面渲染出 0 , 这时候渲染完成，useState就拍了一张快照，count的值就固定为0了， 
点击触发handlClick ,理想中应该打印1，甚至setTimerout异步 2秒后打印，也是0 。

之所以页面上的 0 变成1 ，是一因为state变化了，组件重新渲染， 把修改后的count值1 放进去了，
这时候useState 又进行拍照，第二张快照了，再次点击 打印 2个 1

至于 setTimeout 不管延迟几秒打印，它找的都是App() 这个作用域下的count,因为快照已经固定了，
多次setCount,和延迟打印，值都是固定的


```

![image-20231119175621074](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231119175621074.png)

## 12.状态队列和自动批处理

+ React 会等到事件处理函数中的代码都执行完毕，再处理state更新，队列中的state更新都处理完了，再进行UI的更新

  这就是自动批处理

```
import { useState } from "react"
function App() {
  let [count, setCount] = useState(0)
  const handlClick = ()=>{
     setCount(count+1)   
     setCount(count+1)
     setCount(count+1)
     console.log(count);
     setTimeout(()=>{
     setCount(count+1)
     console.log(count);
     },2000)
  }
  console.log(`重新渲染了`);
  return (
    <>
      <div>header</div>
      <button onClick={handlClick}>+</button>
      {count}
    </>
  )
}
一共调用了4次setCount  , 页面值重新渲染2次 ，第一次 同步的3个setCount，第二次是异步调用 setCount 自动批处理
```

![image-20231119181247875](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231119181247875.png)

```
setCount 回调函数的写法 和 count+1 写法的区别


import { useState } from "react"
function App() {
  let [count, setCount] = useState(0)

  const handlClick = ()=>{
    //  setCount(count+1) 0 + 1    # 这个count 都是 App函数内部作用域下的，找都都是同一个 count 变量
    //  setCount(count+1) 0 + 1
    //  setCount(count+1) 0 + 1
     setCount( (c)=> c+1 ) //0 +1  # c 这个形参 来之React核心处理函数中，作用域再React下，每次都返回上一次计算后的值
     setCount( (c)=> c+1 ) //1 + 1 # 这个 c 形参 和count 没任何关系
     setCount( (c)=> c+1 ) // 1+ 2
     console.log(count)				# 快照特性  count 打印还是0 。
     setTimeout(()=>{
      
       console.log(count);         # 快照特性  count 打印还是0 。

     },2000)
  }
  console.log(`重新渲染了`);
  return (
    <>
      <div>header</div>
      <button onClick={handlClick}>+</button>
      {count}
    </>
  )
}

页面显示 3,  3次 setCount 都执行完后， 还是只重新渲染了一次   自动批处理

```

![image-20231119182546381](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231119182546381.png)



## 13  useState修改状态要只能通过 set**() 自定义修改器

###  对数组和对象的操作需要注意事项

![image-20231119194824992](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231119194824992.png)

```
# 数组的常用操作

import { useState } from "react"
import { cloneDeep } from "loadsh"   //深拷贝的插件
function App() {
  let [list, setList] = useState( [
    {id:1,value:"aaa"},
    {id:2,value:"bbb"},
    {id:3,value:"ccc"}
  ])

  const handlClick = ()=>{
    // setList([...list,{id:4,value:"dddd"}])  // 在最后添加
    
    // setList([{id:4,value:"dddd"}]，...list) // 在最后添加
    
    //setList([...list.slice(0,1), {id:4,value:"dddd"}, ...list.slice(1) ]) // 在指定位置添加
    
   // let newList = [...list];    //数组倒序， 先拷贝一份副本   这里是浅拷贝，层级深的数组需要深拷贝
      let newList = cloneDeep(list)
      newList.reverse();
      setList(newList)
    
    
    
  }
  return (
    <>
      <div>header</div>
      <button onClick={handlClick}>+</button>
      <ul>
        {
          list.map((item)=>{
            return <li key="item.id">{item.value}</li>
          })
        }
      </ul>
    </>
  )
}
```

### 对象的常用操作

```

import { useState } from "react"
import { cloneDeep } from "loadsh"   //深拷贝的插件
function App() {
  let [userInfor, setInfor] = useState({
        userName:{
          firstName:"zhang",
          lastName:"三"
        },
        age:20   
      })

  const handlClick = ()=>{
    // setInfor({                  //修改单项  浅层 数据
    // ...userInfor,
    // age:22
    // })
    
    setInfor({                    //修改层级较深的属性就很麻烦
      ...userInfor,				  //这个写的很恶心	
      userName:{
        ...userInfor.userName,
        firstName:"li"
      }
    })
    
   let newInfor = cloneDeep(userInfor);  //通过深拷贝的方式，复制一个副本，在修改。 深拷贝，递归，性能消耗大。
    newInfor.userName.firstName="li"；   //我只是项修改一个属性，还要递归，复制。
    setInfor(newInfor)
  }  
  return (
    <>
      <div>header</div>
      <button onClick={handlClick}>+</button>
      {JSON.stringify(userInfor)}
    </>
  )
}
```

### immer  简化不可变数据的操作

```
#immer 是什么
 比cloneDeep 深拷贝来说， immer 只修改要改变的值，未改变的属性公用内存中存在的旧值， 类似把未改变的属性嫁接过来复用
 使用的是 proxy 代理 ，跟vue3一样	
 yarn add immer use-immer
 
#useImmer  要求用回调函数的方式操作数据  useImmer 操作数组
import { useImmer } from 'use-immer'
function App() {
  let [list, setList] = useImmer([
    { id: 1, value: 'aaa' },
    { id: 2, value: 'bbb' },
    { id: 3, value: 'ccc' },
  ])

  const handlClick = () => {
    setList((draf) => {
      draf.push({ id: 4, value: 'ddd' })
      draf[0]={id:1,value:'hhh'}
    })
  }
  return (
    <>
      <div>header</div>
      <button onClick={handlClick}>+</button>
      <ul>
        {list.map((item) => {
          return <li key={item.id}>{item.value}</li>
        })}
      </ul>
    </>
  )
}
# useImmer 操作多层对象
 ...省略
  let [userInfor, setInfor] = useImmer({
    userName:{
      firstName:"zhang",
      lastName:"三"
    },
    age:20   
  })

  const handlClick = () => {
    setInfor((draf) => {
     draf.userName.firstName="李"
    })
  }
 ...省略 
```

## 14 惰性初始化useState 的初始值

```
# 需求 加入useState(computed(0))  这个初始值需要通过一个复杂的计算获得
import { useState } from "react"

function computed(n){
  console.log("vvvv");
  return n+1+2+3;
}

function App() {
 // let [count,setCount] =useState(computed(0))  //这样写，每次点击都会执行computed函数，打印vvv
 
 //惰性初始化,只会在初始化初始值的时候打印一次vv,后续点击不会触发  性能优化的一种方式 
 let [count,setCount] =useState( () => computed(0))  
  const handlClick = () => {
    setCount(count+1)
  }
  return (
    <>
      <div>header</div>
      <button onClick={handlClick}>+</button>
      {count}
    </>
  )
}

export default App
```

## 15 状态提升来解决共享问题

+ 多次渲染同一个组件，每个组件都有自己的state,且不是共享的。
+ 说白了状态提升就是把数据提到公共的父组件上申明， 父组件传递属性， 子组件通过props 接受， 这样实现数据共享

 ```
 function MyButton(){
   let [count,setCount] =useState(0)
   const handlClick = () => {
     setCount(count+1)
   }
   return (
     <>
       <button onClick={handlClick}>+</button>
       {count}
     </>
   )
 }
 function App() {
   return (
     <>
       <br/>
       <MyButton></MyButton>    // 同一个组件 不同的实例的state都是独立，互不影响。
       <br/>
       <br/>
       <MyButton></MyButton> 
     </>
   )
 }
 
 ```

![image-20231119210815103](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231119210815103.png)

```
#所谓的状态提升就是取了个高大上的名字， 说白了就是父组件分发 属性和事件

import { useState } from "react"
//子组件
function MyButton({ count, handlClick}){
  return (
    <>
      <button onClick={ handlClick }>+</button>
      {count}
    </>
  )
}
//父组件
function App() {
  let [count,setCount] =useState(0)   //在父组件中定义state
  const handlClick = () => {
    setCount(count+1)
  }
  return (
    <>
      <br/>
      <MyButton count={count}  handlClick={ handlClick }></MyButton>  #子组件公用同一个状态
      <br/>
      <br/>
      <MyButton count={count}  handlClick={ handlClick }></MyButton> 
    </>
  )
}
```

## 16 组件状态重置处理的几种情况

```
1. 第一种情况 showBtn  控制是否创建还是销毁 button 其实准确的应该说是否创建这个button vdom
function MyButton(){
  let [count,setCount] =useState(0)
  const handlClick = () => {
    setCount(count+1)
  }
  return (
    <>
      <button onClick={handlClick}>+</button>
      {count}
    </>
  )
}
function App() {
  let [showBtn,setShow] =useState(true)
  let changeShow = () =>{
    setShow(!showBtn)
  }
  return (
    <div>
      <button onClick={ changeShow }>show</button>
      {
        // 销毁组件，状态重置为0.  
        showBtn ? <MyButton/> : null
      } 
    </div>
  )
}

```

![image-20231119212642446](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231119212642446.png)

```
# 第二种情况
 当showBtn 为true的时候，显示button,并添加红色边框, false 就显示没用边框的button , 
 这两个button是不同的button,但是位置不变  所以状态没有重置
 
import { useState } from "react"

function MyButton({ style }){
  let [count,setCount] =useState(0)
  const handlClick = () => {
    setCount(count+1)
  }
  return (
    <>
      <button style={style} onClick={handlClick}>+</button>
      {count}
    </>
  )
}
function App() {
  let [showBtn,setShow] =useState(false)
  let changeShow = () =>{
    setShow(!showBtn)
  }
  return (
    <div>
      <button onClick={ changeShow }>show</button>
      <br/>
      <br/>
      {
        showBtn ? <MyButton style ={{ border:'1px solid red'}}/> : <MyButton/>
      } 
    </div>
  )
}
 //不同的key ,或者包裹一层不同的元素，都会重置状态。因为react 认为这两个button 位置和结构不同了
<MyButton style ={{ border:'1px solid red'}} key="aa"/> : <MyButton key="bb"/>
```

![image-20231119213455092](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231119213455092.png)

![image-20231119213502864](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231119213502864.png)

## 17. 通过state 状态生成计算变量  类似vue的计算属性

```
import { useState } from "react"
function App() {
  let [count, setCount] = useState(0)
  let doubleCount = count * 2    // 非常简单就实现了计算属性	
  const handlClick = ()=>{
     setCount(count+1)
  }
  return (
    <>
      <div>header</div>
      <button onClick={handlClick}>+</button>
      {count}
      { doubleCount }
    </>
  )
}
```

![image-20231119214637620](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231119214637620.png)

## 18 受控组件与 非受控组件

+ 通过props 控制的组件称为 受控组件
+ 通过state 状态控制的组件 称为 非受控组件
+ React 表单内置了受控组件的行为
  value+onChange     //输入框， 多行文本输入等受控组件
  checked +onChange  // 单选框， 下拉菜单等受控组件

```
# input 表单控件就是一个典型的受控组件

import { useState } from "react"
function App() {
  
   let  [inputValue,setInputValue] = useState('')

   let handChange=(e)=>{
    setInputValue(e.target.value)
   }
  return (
    <>
      <div>
        <input type="text" value={inputValue} onChange={handChange} />
      </div>  
    </>
  )
}

```

## 19 什么是hooks

+ useState useEffect  等等以use开头的功能性函数
+ 想要实现什么引入对应的hooks 函数， 像插板一样， 可拔插。使用非常方便
+ hooks分类   ： react 内置hooks  自定义hooks  第三方hooks

### 19.1useRef() 的用法

```
import { useState,useRef } from "react"
function App() {
   let [count,setCount]=useState(0)
   let num=0;
   let handClick=()=>{
   // setCount(count+1)   #不修改state 状态，正常打印123456，每次加1
    num++;				 # 解除注释后，state每点击一次，触发更新，重新执行 App()函数， num也重置为0， 打印都是1	
    console.log(num);
   }
  
  return (
    <>
      <button onClick={handClick}>+</button>
      -- {count}
    </>
  )
}
需求 如何让 每一次更新时候，记住num属性的值
.... 
   let num = useRef(0);
   let handClick=()=>{
    setCount(count+1)
    num.current++;
    console.log(num.current);
   }
.....

#应用场景  
1 操作dom,例如让一个节点获取焦点，滚动到指定位置

function App() {
   let myRef = useRef(null)
   let handClick=()=>{
    //打印dom
    console.log(myRef.current);
   }
  return (
    <>
      <button onClick={handClick}>+</button>
      <div ref={ myRef }></div>
    </>
  )
}



2清楚指定的定时器，防止定时器累加， 因为每一次触发更新，重新执行的App()函数，作用域都不同，

....省略代码
  let handClick=()=>{
    setCount(count+1)
    clearInterval(timer.current) ; 
    timer.current=setInterval(()=>{
      console.log("启动定时器");
    },1000)
   }
....省略代码   
```

### 19.2useRef 和 useState 的区别

![image-20231119222732819](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231119222732819.png)

### 19.3ref 转发

 当把 ref 属性写在组件上的时候，就需要进行 forwardRef 

```
import { useState,useRef , forwardRef} from "react"

# 使用forwardRef 进行转发的写法 参数必须有props ,ref 形参
const DeomRefCom = forwardRef ( function DeomRefCom(props,ref){
  let [inputValue,setValue]=useState('')
  let handleChange = (e)=>{
      setValue(e.target.value)
  }
  return (
    <div>
      我是子组件
      <input type="text" ref={ref} value={inputValue} onChange={ handleChange } />
    </div>

  )
})
function App() {
  let inputRef =useRef(null)
  let consoleClick=()=>{
    console.log(inputRef.current);   #点击就能拿到 input 元素
    inputRef.current.focus();		#下面就可以操作dom		
    inputRef.current.style.backgroundColor="pink"
  }
  return (
    <>
     <div> ref 转发 forwardRef</div>
     <button onClick={consoleClick}>打印ref</button>
     <DeomRefCom ref={ inputRef}></DeomRefCom>    #ref 使用在组件身上
    </>
  )
}

```

### 19.4

```
useImperativeHandle(ref,()=>{
	return {           #useImperativeHandle 可以自定义暴露的属性或者 函数，
						                    封装组件的时候不想完全暴露DOM
	 ...				                    只给调用者提供指定的自定义函数
	 ...	
	}
})

import { useState,useRef , forwardRef,useImperativeHandle} from "react"

const DeomRefCom = forwardRef ( function DeomRefCom(props,ref){
  useImperativeHandle(ref,()=>{  #父组件的ref 被转发到了useImperativeHandle 这个hook函数上了
    return {
      fn(){                     #自定义要暴露的fn函数
        console.log(ref);
        console.log("自定义的fn");
      },
      changeInput(){            #自定义要暴露的changeInput函数   里面操作了input元素 
        inpRef.current.focus();
        inpRef.current.style.backgroundColor="pink"
      },
      hh:"自定义变量hh"
    }
  })
  let inpRef = useRef(null)
  let [inputValue,setValue]=useState('')
  let handleChange = (e)=>{
      setValue(e.target.value)
  }
  return (
    <div>
      我是子组件   inpRef是子组件内部自己定义的ref
      <input type="text"ref={inpRef} value={inputValue} onChange={ handleChange } />
    </div>

  )
})
function App() {
  let inputRef =useRef(null)
  let consoleClick=()=>{
    inputRef.current.fn();
    inputRef.current.changeInput();
    console.log(inputRef.current.hh);
  }
  return (
    <>
     <div> ref 转发 forwardRef</div>
     <button onClick={consoleClick}>打印ref</button>
     <DeomRefCom ref={ inputRef}></DeomRefCom>
    </>
  )
}
```

![image-20231120143509331](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231120143509331.png)

## 20 useEffect

**react希望组件都按照 纯函数的标准进行实现， 但实际开发项目的时候避免不了 获取数据，操作dom，等一些事件， 这些事件的调用所产生的副作用针对的是 纯函数这个标准来的 ，下面这两个概念是互斥的**

+ 纯函数的概念

    只负责自己的任务，调用时候不会更改 之前就存在的变量或者对象
    给定相同的输入， 输出必定相同。

+ 副作用的概念   总觉得副作用翻译的很死板 emo

  函数在执行的过程对外部造成的影响称为 副作用。 如 Ajax 调用获取数据，DOM操作等等
  react组件事件操作 是可以处理副作用的，但不是所有副作用都需要手动触发事件来解决，有时候需要初始化数据（副作用），

  useEffect可以在页面渲染是调用 axios 获取数据



```
# 我是一个例子  类似点击，选择下拉菜单等等事件 。初始化获取数据就需要主动触发，而不是等待事件触发

function App() {
  let keyRef = useRef(null)
  // setTimeout(()=>{  不规范的写法
  //   keyRef.current.focus()
  // },2000)

  let getFocus =()=>{  //通过事件处理 副作用
    keyRef.current.focus()
  }
  useEffect(()=>{  // useEffect 类似 vue 的 onMounted 生命周期 , JSX渲染完成后触发
    // getData() 获取数据
    keyRef.current.focus()
  })
  return (
    <>
     <button onClick= { getFocus} >获取焦点</button>
     <input type="text" ref={ keyRef } />
    </>
  )
}


```

### 20.1 useEffect 的执行顺序

```
function App() {
  console.log(1);
  let [count,setCount]=useState(0)
  let handlClick =()=>{  //通过事件处理 副作用
   console.log(2);
   setCount(count+1)
  }
  useEffect(()=>{
    // getData() 获取数据
    console.log(3);
  })
  return (
    <>
     <button onClick= { handlClick} > + </button>
     <br/>
     {count}
     {console.log(4)}
    </>
  )
}

打印 1 4 3    2 不触发肯定不打印  点击后又 打印 1 4 3
说明组件jsx 渲染完后才触发 useEffect, 每次更新都会触发。
```

![image-20231120151236982](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231120151236982.png)

### useEffect 依赖项控制effect 是否执行

```
## 需求  数据初始化只需要进行一次，不可能每次更新组件都调用初始化数据的函数
		useEffect() 可以写多个，分别处理不同的副作用， [] 依赖项可以控制那个 effect 执行
		
		[]依赖项数组底层实现是 Object.is()函数实现的   
		//Object.is(1,1) true, 不执行, 因为依赖的值没发生变化
		//Object.is(1,2) false, 执行, 因为依赖的值发生变化	
        
//只会执行一次
useEffect(()=>{
     getData() 获取数据
  },[])
# []  依赖项数组， 函数中用到了useState 生命的状态，或者计算变量， props传递的变,都要写进数组中

useEffect(()=>{
    consloe.log(count)   //state 状态
  },[count])

-------------------------------------------------------------------------------------
let doubleCount = count * 2
useEffect(()=>{
    consloe.log(doubleCount)   //计算属性 状态
  },[doubleCount])
  
  
```

### 20.2 useCallBack 处理依赖项

```
let [count,setCount]=useState(0)
//错误写法
let fn = ()  > {
	setCount(count+1)
	consloe.log(count)  
} 

useEffect(()=>{					//因为fn 函数中用到了count ,fn 函数便成了依赖项
     fn()						//这样写eslint 是会报错的
  },[fn])

# 原因 Object.is(function,function)  看着是两个相同的函数，因为内存中地址不一样，返回的是false, 不管count是否改变
  都会执行这个 useEffect
  
 //这样写才正确 useCallBack((),[]) 也是有依赖项的  ，就是写起来太恶心，麻烦
 ------------------------------------------------------------------------
  let fn = useCallBack( ()=> {       
	setCount(count+1)
	consloe.log(count)  
	},[count])	
  
  最好的方式的把函数定义在 useEffect 中  逻辑清晰，写法简单
  
  useEffect(()=>{
  	fn(){
    setCount(count+1)
	consloe.log(count)  
  	}
  	fn();
  },[count])
  
  
```

### 20.3 useEffect 清理操作的重要性

```
function ChatRoom(){
  useEffect(()=>{
    console.log("进入聊天室");           

    return ()=>{                #在组件被销毁之前调用清理的回调函数
    console.log("退出聊天室");  
    }
  })
  return (
    <div>进入聊天室</div>
  )
}

function App() {
  let [isShow, setStatus] = useState(true)
  let enterInChat= ()=>{
    setStatus(!isShow)
  }
  return (
    <>
     <button onClick= { enterInChat} > { isShow ? "显示" : "隐藏"} </button>
     <br/>
     { isShow && <ChatRoom></ChatRoom> }
    </>
  )
}
```

```
//模拟异步请求数据的函数
let fetchDate = ( chatType) =>{        //根据进入的聊天室不同获取不同的数据
  let timerOut = chatType === "情感聊天室" ? 2000 : 200;
  return new Promise((resolve,reject)=>{
    setTimeout(()=>{
      if(chatType){
        let list= [
          {id:1,tvalue:chatType+"你好吗"},
          {id:2,tvalue:chatType+"爱笑的眼睛"},
          {id:3,tvalue:chatType+"情感专线"}
        ]
        resolve(list);
      }else{
        reject("error")
      }
    },timerOut)
  })
}

# 需求 切换聊天室显示对应的聊天室内容， 
# chatType 的值发生变化， useEffect在组件更新的时候重新执行
# 因为没用执行清理操作， 会导致数据混乱


function ChatRoom({ chatType }){        
  console.log(chatType);
  let [list,setList] =useState([])
  useEffect(()=>{
    async function fn(){
      let list =  await fetchDate(chatType);
       setList(list)
    }
    fn();
  },[chatType])
  return (
    <div>
      <ul>
        { 
          list.map(item =>{
          return  <li key={item.id}>{item.tvalue}</li>
          })
        }
      </ul>
    </div>
  )
}

function App() {  
  let [chatType,setType] =useState("情感聊天室");
  let changeType = (e) =>{
    setType(e.target.value)
  }
  return (
    <>
      <select value={ chatType} onChange={ changeType}>
        <option value="情感聊天室">情感聊天室</option>
        <option value="体育聊天室">体育聊天室</option>
      </select>
     <br/>
     <ChatRoom chatType={chatType }></ChatRoom>
    </>
  )
}
```



![image-20231120220635292](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231120220635292.png)

```
省略相同代码， 重点看useEffect
  useEffect(()=>{
    let isChear=false;
    async function fn(){
      let list =  await fetchDate(chatType);
      if(!isChear){
        setList(list)
      }
    }
    fn();
    return() => {                              #加入清理控制
      isChear=true;
    }
  },[chatType])
```

### 20.4 uesLayoutEffect

+ useEffect()是异步更新，在页面到屏幕之后执行。 可以理解成 vue的 OnMounted 元素已经被挂载在body上了
+ useLayoutEffect（）执行在useEffect()之前，  可以理解成vue 中的 created() 生命周期
  解决的问题是，防止屏幕闪烁。 假如页面渲染一个state动态数据，在useLayoutEffect 中又修改了这个数据，那么不会触发2次渲染，而是拿到最终的结果进行渲染。 

### 20.5 useInsertionEffect

```
 useInsertionEffect  解决方案，前2个都能拿到dom元素, 
 
 useInsertionEffect() 在dom创建之前就执行完了
 
 主要用于css-in-js解决方案。 动态的注入 css样式。
 
 打印结果 3  2  1   

function App() {
  useEffect(()=>{
    console.log(1);
  })
  useLayoutEffect(()=>{
    console.log(2);
  })
  useInsertionEffect(()=>{
   let style =document.createElement("style");
    style.innerHTML=`
    .box{
      font-size:20px;
      color:white;
      background:red;
      width:200px;
      height:200px;
    }
    `
    document.head.appendChild(style)
    console.log(3);
  })
  return (
    <> 
     <div className="box">hello react</div>
    </>
  )
}
```

## 21.useReducer

**如何把同一个State相关的业务逻辑抽离出去，统一进行管理**

**对一个逻辑多，处理函数多的state，我们为了方便管理，可以使用useReducer**

实际开发过程中，不只是简单的修改，删除操作，也不只是一个State,如果都写在组件内部，不是不行，如果按顺序写，

相关State的处理都尽量写在一块，除去组件内的事件处理函数多一点，问题不是太大。

 但为了追求更优秀的代码组织架构，可以使用useReducer

![image-20231121164801112](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231121164801112.png)

```
# useReducer 使用方式
  let [ list, dispatch ] = useReducer( 处理函数，初始数据 )

//处理函数
const handleTodo=(state,action)=>{
  switch(action.type){
    case "edit":
      return  state.map(item=>{
        if(item.id===action.id){
         return  {...item,value:"new"+item.value}
        }else{
          return item
        }
      })
    case "delete":
      return state.filter(item=>{
        if(item.id===action.id){
          return false
        }else{
          return true;
        }
      })  
  }
}
function App() {
  let [list,dispatch] = useReducer( handleTodo,
   [
    {id:1,value:"跑步半小时"},
    {id:2,value:"吃早餐"},
    {id:3,value:"写个人项目"},
   ]
  ) 
  return (                    
     <div>
       <ul>
        {
          list.map((item)=>{
            return <li key={item.id}>
                    {item.value}
                    <button onClick={()=>dispatch({type:"edit",id:item.id})}>编辑</button>
                    <button onClick={()=>dispatch({type:"delete",id:item.id})}>删除</button>
                  </li>
          })
        }
       </ul>
     </div>  
  )
}

也可以这样调用 其他代码省略
 ......
  let handleEdit=(id)=>{
    dispatch({type:"edit",id})
  }
  .....
    <button onClick={()=>handleEdit(item.id)}>编辑</button>
```

```
//其他不变，只是操作数组更方便了
import { useImmerReducer } from "use-immer"
// const isChrome = navigator.userAgent.indexOf('Chrome') > -1
const handleTodo=(state,action)=>{
  let data=null;
  let itemIndex=0;
  switch(action.type){
    case "edit":
      data = state.find(item => item.id===action.id);
      data.value="new"+data.value;
      break;
    case "delete":
      itemIndex = state.findIndex(item => item.id===action.id);
      state.splice(itemIndex,1)
      break; 
  }
}
```

## 22 createContext 和useContext 实现跨级传递

```
看一个问题。   这中显示的props 传递只适合父子组件，或者层级很少。 如果层级太深就不适合了
function App() {
  return ( 
     <div>
       <MyCom1 count="123"></MyCom1>
     </div>  
  )
}
const MyCom1=({ count })=>{
  return (
    <div>
      我是子组件1 ---- { count }
      <MyCom2 count={count}></MyCom2>
    </div>
  )
}
const MyCom2=({count})=>{
  return (
    <div>
      我是子组件2-- {count}
    </div>
  )
}


```

```

import { createContext,useContext } from "react"

const Context = createContext();   #创建Context 规范 首字母大写  createContext(0) 可以添加默认值

function App() {
  return ( 
     <div>
       <Context.Provider value="我是context传递的string">    #包裹要传递的子组件 通过value属性传递
        <MyCom1 count="123"></MyCom1>						#可以传递普通值，state，数组，对象
       </Context.Provider>
     </div>  
  )
}
const MyCom1=()=>{
  return (
    <div>
      我是子组件1 
      <MyCom2></MyCom2>
    </div>
  )
}
const MyCom2=()=>{
  const value =useContext(Context)					#使用context
  return (
    <div>
      我是子组件2-- { value }
    </div>
  )
}
```

## 23memo性能优化

```
初渲染1次， 点击后1次， 其实第二次子组件没发生任何变化，根本不需要重新渲染。
这里子组件哪怕是用到了父组件传递过来的普通值，只要没发生改变，按纯函数的概念，既定的输入，既定的输出。就不需要重新渲染。
除非是父组件状态更新， 子组件也用到了父组件传递的state 数据，这肯定是要更新的。

import { useState } from "react"
function App() {
  let [count,setCount] = useState(0);
  let handleClcik = () => {
    setCount(count+1)
  }
  return ( 
     <div>
      <button onClick={ handleClcik }>+</button>
      <div>{count}</div>
      <MyCom1></MyCom1>
     </div>  
  )
}
const MyCom1=()=>{
  return (
    <div>
      我是子组件
      {console.log("跟着父组件一起重新渲染了")}
    </div>
  )
}

```

![image-20231121190724213](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231121190724213.png)

```
const MyCom1 = memo (function MyCom1(){  //这里没写成箭头函数，而是申明式函数，是因为esLint检查警告
  return (								 //这样就子组件就不会重新渲染
    <div>
      我是子组件
      {console.log("跟着父组件一起重新渲染了")}
    </div>
  )
})
```



```
##需要注意的是传递数组和对象 看着只是修改了count ,msg没修改，  但是子组件依旧跟着父组件一起跟新了
# 这是因为Object.is([],[])  数组和对象比较的是内存中的地址，都不是同一个对象，肯定触发更新

import { useState ,memo,useMemo} from "react"
function App() {
  let [count,setCount] = useState(0);
  let handleClcik = () => {
    setCount(count+1);
    setMsg("world")
    console.log(msg);
  }
   let [msg,setMsg]=useState("hello msg");
   # 这样写即使msg 没修改，子组件也会触发更新  Object.is([],[])  数组和对象比较的是内存中的地址 
   let list=[msg.toLocaleLowerCase(),msg.toLocaleLowerCase()]  #list
   
 // let list = useMemo(()=>{   #useMemo 可以保证同一个数组，值不发生变化，就不会触发更新  正确写法
 //    return [msg.toLocaleUpperCase(),msg.toLocaleUpperCase()]
 //   },[msg])

  return ( 
     <div>
      <button onClick={ handleClcik }>+</button>
      <div>{count}</div>
      <MyCom1 list={ list } ></MyCom1>
     </div>  
  )
}
const MyCom1 = memo(function MyCom1({ list }){

  return (
    <div>
      我是子组件 
      <ul>
        {
          list.map((item)=>{
            return <li>{item}</li>
          })
        }
      </ul>
      {console.log("跟着父组件一起重新渲染了")}
    </div>
  )
})
```

## 24 flushSync 确保dom立刻更新

react中的渲染是异步的，多个state状态的更改会被自动批处理完成，然后进行一次更新。但是实际开发过程中，有中需求。

在更新了某个state,需要立刻拿到最新的dom，在进行一些操作。  vue中是 # nextTick()

```
import { useRef } from "react"
import { useState } from "react"

function App() {
   let [count,setCount] = useState(0)
   let [msg,changeMsg] = useState("我是msg")
   let h2Ref = useRef(null);
   let handleSet = ()=>{
      setCount((count)=> count+1)          #修改2个state状态， 批处理，打印一次
      changeMsg("changeMsg")
      console.log(h2Ref.current.innerHTML)
   }
  return ( 
     <div>
       <button onClick={handleSet}>+</button>  
       <h2 ref={h2Ref}>{count}</h2>
     </div>  
  )
}

## flushSync() 会打破react 修改state，自动批处理的机制。 如图，修改了2次state, 打印了2次
 let handleSet = ()=>{
      flushSync(()=>{
         setCount((count)=> count+1)
         //这里count 还是0, 所以 flushSync() 立马更新了dom
         console.log("第1次渲染"+h2Ref.current.innerHTML)
      })
      
      changeMsg("changeMsg")
      //这里打印count 是1  说明dom更新了， 不再是异步更新渲染
      console.log("第2次渲染"+h2Ref.current.innerHTML)
   }


```

![image-20231122210735672](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231122210735672.png)

## 25 errorboundary（错误边界）

**react 页面由很多个组件组成，当一个页面报错，整个页面都空白了，这样肯定是不符合开发人员心里预期，上线了对用户体验也不好。不可能因为一个组件出错了，其他展示信息用户也看不到。**

```
yarn add react-error-boundary   #安装

import { ErrorBoundary } from "react-error-boundary"  #引入

function App() {

 //fallback  中可以自定义出错了的组件
  return ( 
     <div>
         <h1>hello react</h1>
         <ErrorBoundary fallback={ <div>children render error</div> }>  #使用
            <ChildrenCom></ChildrenCom>
         </ErrorBoundary>
     </div>  
  )
}

function ChildrenCom(){
   console.lg("...")
   return (
      <div>我是子组件</div>
   )
}

```

![image-20231122205311594](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231122205311594.png)

## 26 懒加载组件 lazy,Suspense



## 27 startTranstion 并发渲染

+ React 18以前，渲染是一个 单一、不间断、同步的事务，一旦组件开始渲染就不能被中断。

+ startTranstion 可以把一个任务的优先级降低。

如图需求，根据用户的输入，把关键字标记为红色

![image-20231130215909092](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231130215909092.png)

```
以下 代码中  setValue(e.target.value)    setQuery(e.target.value)  //这两个默认都是紧急任务
 用户输入的时候    Content子组件会根据输入内容进行匹配，修改1万个li 标签中对应的样式。 触发渲染更新组件
 
 而输入框也要显示用户输入的内容， 也要进行渲染最新的内容。 
 同时都在渲染， 但是根据关键字匹配，再到更新1万个li 标签中的span样式，
 明显消耗时长比input 输入框渲染消耗时长多
 
 这就导致了用户再输入的时候 下面的样式发生了变化，而输入框中的输入内容是卡顿，延迟渲染的。
 这样的体验不是很好
function Content ({query}) {
 
  let items = []
  let text = 'hello world'
  if (query !== '' && text.includes(query)) {
    const arr = text.split(query);
    console.log(arr[0]);
    for (let i = 0; i < 10000; i++ ) {
      items.push(<li key={i}>
        {arr[0]}
        <span style={{color:"red"}}>{query}</span>
        <span>{arr[1]}</span>
        </li>)
    }
  } else {
      for (let i = 0; i < 10000; i++ ) {
        items.push(<li key={i}>{text}</li>)
      }
  }
  
  
  return (
      <ul>
        { items }
      </ul>
  )
}

const App = () => {
  let [search, setValue] = useState('')
  let [query, setQuery] = useState('')
  let handleChange = (e) => {
    setValue(e.target.value)      //这两个默认都是紧急任务
    setQuery(e.target.value)
  }
  return (
    <div>
     hello App  <input type="text" value={search} onChange={handleChange} />
      <Content query={query}></Content>
    </div>
  )
}

```

```
startTransition 把耗时较长的任务的优先级降低，先把紧急的任务，用户第一时间能感受到的处理了，再处理非紧急任务

import {startTransition } from 'react'

let handleChange = (e) => {
    setValue(e.target.value)
    
    startTransition(()=>{   //降低优先级，先显示input输入的内容，再触发匹配更新样式。
      setQuery(e.target.value)
    })
  }
  
```

### useTransition 和 useDeferredValue

```
import { useState, useDeferredValue } from 'react'
const App = () => {
  let [search, setValue] = useState('')
 
  let query = useDeferredValue(search)    //最方便省事的用法， 和上面使用startTranstion效果一样
 
  let handleChange = (e) => {
    setValue(e.target.value)
  }
  
 return (
    <div>
     hello App  <input type="text" value={search} onChange={handleChange} />
       {pengding && <div>loading</div> }
      <Content query={query}></Content>
    </div>
  ) 
```

```
import { useState,startTransition,useTransition} from 'react'
const App = () => {
  let [search, setValue] = useState('')
  let [query, setQuery] = useState('')
  
  //不实用，可以加一个变量，在上一个任务没结束时候，显示加载状态，当然也可以处理一些自己的逻辑需求
  let [pending,startTransition] =useTransition("");  ，
  
  let handleChange = (e) => {
    setValue(e.target.value)   //用户在输入时候，非紧急任务处于 pending 状态

    startTransition(()=>{
      setQuery(e.target.value)
    })
   
  }
  return (
    <div>
     hello App  <input type="text" value={search} onChange={handleChange} />
       {pengding && <div>loading</div> }  ##显示loading
      <Content query={query}></Content>
    </div>
  )
}  
```

