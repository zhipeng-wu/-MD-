## 安装

```javascript
yarn add pinia 或者 npm install pinia
```



## 使用

```javascript
#1 引入pinia
import { defineStore } from pinia

#2 导出 并 定义store

#defineStore（） 第一个参数 是 sid  数据从库的唯一id ，
export const useAltersStore = defineStore('alters',{
    ...store配置
})

#tips  
命名规则 use + 仓库名称   后面的参数取仓库名称
如 useConterStore    defineStore("count")
   useMusicStore     defineStore('music')


```

```javascript
#options参数 写法
defineStore() 的第二个参数可接受两类值：Setup 函数 或 Option 对象。

const useCounterStore = defineStore("count",{
    state: () => {
        count:0
    },
    getters:{
        doubleCount:(state) => state.count*2
    },
    actions:{
        increment(){
            this.count++
        }
    }
})

#你可以认为 state 是 store 的数据 (data)，
getters 是 store 的计算属性 (computed)，
而 actions 则是方法 (methods)。

#为方便上手使用，Option Store 应尽可能直观简单。
```

```javascript
# setup参数 写法
import {ref,computed} from "vue"
import {defineStore} from 'pinia'

const useCounterStore = defineStore("count",()=>{
    const count =ref(0)
    const doubleCount =computed(()=> count.value * 2)
    const increment=()=>{
        return count.value++
    }
    return {count, doubleCount, increment}
})
#setup 写法中
#ref() 就是 state 数据
#computed() 就是 getters
#定义的increment 函数 就是 actions

优点: Setup store 比 Option Store 带来了更多的灵活性，
	 因为你可以在一个 store 内创建侦听器，并自由地使用任何组合式函数.
缺点: 使用组合式函数会让 SSR 变得更加复杂。     
```

## storeToRefs

```javascript
#store 是一个用 reactive 包装的对象，这意味着不需要在 getters 后面写 .value

export const useCounterStore = defineStore('counter', {
  state: () => { 
    return {   #定义了2个属性
      count: 0,
      firstName:"张"
    }
  },
  getters: {
    doubleCount:(state)=> state.count * 2
  },
  actions: {
    increment() { 
      this.count++
    }
  }
})

在页面中一般我们都可以用 . 的方式访问属性
const counter = useCounterStore();
const btnClick = () => {
  counter.firstName = "李"
  counter.count++
}

如果想把每一个属性都解构出来，还保持数据的响应性 需要用到storeToRefs
import { storeToRefs } from 'pinia';
const counter = useCounterStore();
const { count, firstName } = storeToRefs(counter)
const btnClick = () => {
  firstName.value = "李"
  count.value++
}
```



## state

### 定义State

```javascript
import { defineStore } from 'pinia'

const useStore = defineStore('storeId', {
  // 为了完整类型推理，推荐使用箭头函数
  state: () => {
    return {
      // 所有这些属性都将自动推断出它们的类型
      count: 0,
      name: 'Eduardo',
      isAdmin: true,
      items: [],
      hasChanged: true,
    }
  },
})
```

```javascript
支持 TypeScript 语法定义 store
import { defineStore } from "pinia"

interface UserInfo {
    name:string,
    age:number  
}
interface State{
    userList: userIfo[],
    user:userInfo | null    
}
const useUserStore =defineStore('user',{
    state: ():State => {
        return {
            userList,
            user  
        }
    }
})

```

### 访问修改state

```javascript
import { useCounterStore } from '@/stores/counter';
const store=useCountStore();
#1第一种方式单属性访问
store.count++
store.increment()

#2通过$patch 一次变更多个属性
store.$patch({
  count: store.count + 1,
  firstName:"张"
})

#3如果修改的是一个集合类型
store.$patch((state) => {
  state.items.push({ name: 'shoes', quantity: 1 })
  state.hasChanged = true
})

```

###  重置state状态

```javascript
#options 选项卡写法 可以直接通过

import { useCounterStore } from '@/stores/counter';
const store=useCountStore();
store.$reset();   //重置 

------------------------------------------------------------------------
#在 Setup Stores 中
export const useCounterStore = defineStore('counter', () => {
  const count = ref(0)
  //手动定义	
  function $reset() {
    count.value = 0
  }

  return { count, $reset }
})

```

### mapState 映射state属性到计算属性

```javascript
import { mapState } from 'pinia'
import { useCounterStore } from '../stores/counter'

export default {
  computed: {
    // 可以访问组件中的 this.count
    // 与从 store.count 中读取的数据相同
    ...mapState(useCounterStore, ['count'])
    // 与上述相同，但将其注册为 this.myOwnName
    ...mapState(useCounterStore, {
      myOwnName: 'count',
      // 你也可以写一个函数来获得对 store 的访问权
      double: store => store.count * 2,
      // 它可以访问 `this`，但它没有标注类型...
      magicValue(store) {
        return store.someGetter + this.count + this.double
      },
    }),
  },
}
```

## getters

```javascript
#getter 一般仅仅依赖state  也可通过this 访问store实例
export const useStore = defineStore('main', {
  state: () => ({
    count: 0,
  }),
  getters: {
    // 自动推断出返回类型是一个 number
    doubleCount(state) {
      return state.count * 2
    },
    //如果使用了typeScript 返回类型**必须**明确设置  
    doublePlusOne(): number {
      // 整个 store 的 自动补全和类型标注 ✨
      return this.doubleCount + 1
    },
  },
})

如果要访问其他store 中的getter  ，先引入，实例化后在访问就是了

#1引入
import { useOtherStore } from './other-store'

export const useStore = defineStore('main', {
  state: () => ({
    // ...
  }),
  getters: {
    otherGetter(state) {
        #2实例化
      const otherStore = useOtherStore()
      return state.localData + otherStore.data
    },
  },
})

在页面中使用getters 引入store ，实例化后，直接.


```

## actions

action 可以是异步的，能使用async await  ，只要其他返回的是promise ,你甚至不需要使用axios ,使用原生fetch()函数

```javascript
export const useUsers = defineStore('users', {
  state: () => ({
    userData: null,
    // ...
  }),

  actions: {
      //用户注册
    async registerUser(login, password) {
      try {
        this.userData = await api.post({ login, password })
        showTooltip(`Welcome back ${this.userData.name}!`)
      } catch (error) {
        showTooltip(error)
        // 让表单组件显示错误
        return error
      }
    },
  },
})
```

如果需要调用其他store的action  ，同上getter 一样，先引入，再实例化

###  订阅action

