## 组件
- 将一个页面分割成若干个部分，一个页面有js+css+html，将自己的内容分割出来，方便我们的开发，更好维护我们的代码，每个组件封装自己的js+css+html，解决样式命名冲突（好处）。
- 组件分类
  - 页面级别组件：页面
  - 基础组件：页面中的一部分
- 组件的目的：为了实现复用

## 指令&&组件——都有增加功能的特点
- 指令：在标签内增加的行内属性，实现功能的
- 组件：是一个自定义的（扩展）标签<hello></hello>,封装代码的，组件的标签名不能破坏原有的html标签

## 组件：全局组件 | 局部组件
- 全局组件特点：不需要引用(
  > 正常开发中不会使用全局组件，写一些新的包，或者一些全局的功能：loading，可以使用全局组件)
- 局部组件特点：需要引用

## 全局组件
- 特点：1、在哪里都可以调用；2、不用注册
- 包含部分：
  - 结构 html
  - 行为数据 methods，data
  - 样式（现在还未被分离） .vue文件
- 命名
  > 一般情况下html使用短横线隔开命名法，js中使用驼峰，上下一样即可
- 组件
  > 在vue中一个对象就是一个组件

- 使用组件的步骤
  > 1、声明组件并且引入到当前页面；
  2、在组件的父级模板中调用这个组件

- 组件中data是函数
  > 原因：对象可以在外面定义，为了保证组件的独立性，所以定义为函数，形成一个新的作用域，受到保护，data函数中return的是一个对象{}

- 举例
```
<div id="app">
    <my-hi></my-hi>
</div>

Vue.component('my-hi',{
       template:'<h1>{{msg}}</h1>',//只能有一个root element，而且不能直接放置文本节点
        data(){
           return{msg:'hello'}//这个对象可以用来定义数据
        },
    });
```
## 局部组件
- 使用的顺序有三点
  - 引入
  - 注册组件
  - 使用组件（不能和标签相同）
  
- 使用局部组件必须注册，否则不能使用

- 举例
```
<div id="app">
    <my-hi></my-hi>
    <beautiful></beautiful>
</div>

let handsome={
        template:'<h1>{{msg}}</h1>',
        data(){return {msg:'hello'}}
    };
    let beautiful={
        template:'<h1>{{msg}}</h1>',
        data(){
            return {msg:'beautiful'}
        }
    };
    let vm=new Vue({
        el:'#app',
        components:{
            myHi:handsome,
            beautiful,
        }
    })
```
## 组件之间的通信 ：1、父子；2、子父；3、兄弟之间
## 1、父传子
### 传递方式
- 默认组件是独立的，相互不能引用数据，可以通过**属性**的方式传递给儿子
  - 在子组件上添加自定义属性：`<child :m="money" o="美女"/>`
  - 子组件通过props接收变量（如果是多个可用数组的形式）：`props:['m','o']`
  - 将变量放到子组件需要的位置：`<div>child {{m}}{{b}}</div>`
  ```
  <div id="app"></div>
  
  let vm=new Vue({
          el:'#app',
          data:{money:100},//根实例数据都是对象，组件中都是函数
          template:'<child :m="money" o="苹果"/>',
          components:{
              child:{
                  //如果传递多个，数组中可以写多个
                  props:['m','o'],//这个m会挂载在儿子的实例上，相当于挂在了data上;父亲的数据儿子不能更改
                  computed:{
                      b(){return '大'+this.o}//this代表的是当前组件的实例
                  },
                  template:'<div>child {{m}}{{b}}</div>'
              }
          }
      })
  ```
### 校验
- 对象形式的props可以进行校验
- 如果校验不通过，也会渲染到页面，只是给开发者一些提示
- 如果传递的是布尔或者数字，需要加：，否则会被认为是字符串
   ```
   template:'<child :m="money" o="苹果"/>',
   money是变量，o是字符串
   ```
- 四种校验
  - type：检测值的类型，如需多种类型，可以写成数组形式
  - default：属性没有传递时会采用默认属性（m=''这也是传递了，只是为空）
  - required：必须传递，不能和default连用
  - validator：自定义校验器
  ```
  components: {
              child: {
                  props: {
                      m: {
                          type: [Number, Boolean, Array, String],
                          //default:'500',//属性没有传递时会采用默认属性，m='',这也是传了，只是值为空
                          required:true,//必须传递，不能和default连用
                      },
                      o: {
                          validator(val){//val代表传递过来的值，自定义验证器
                              return val==='苹果'
                          }
                      }
                  },//对象形式的props可以进行校验
                  computed: {
                      b() {
                          return '大' + this.o
                      }
                  },
                  template: '<div>child {{m}}{{b}}</div>'
              }
          }
  ```

## 2、子传父
- 子传父通过的是发布订阅：父亲声明一个方法，儿子触发父亲的方法
- $emit->@方法=‘父组件的方法’
### 传递方式
- 父亲声明一个方法
- 儿子绑定一个事件
- 儿子触发这个方法

  ```
  let vm=new Vue({
                                          //this.$on('child',say)
          template:'<div>父{{say}}<child @child="say"></child></div>',
          methods:{
              say(data){
                  console.log(data);
              }
          },
          components:{
              child:{
                  created(){
                      this.$emit('child',this.msg)
                  },
                  data(){
                      return {msg:'饿了'}
                  },
                  template:'<div>子{{msg}}</div>',
  
              }
          }
      });
          vm.$mount('#app');//手动将vue挂载在app上
  ```
## 3、双向绑定
- 儿子触发父亲的一个方法，将最新的值传递给父亲，父亲更改后，属性重新传递，儿子会刷新
### 传递方式
- 父亲将数据传给儿子
- 儿子接收渲染
- 儿子触发父亲的方法
- 父亲改变数据并重新传递给儿子
```
let vm=new Vue({
        el:'#app',
        template:'<div>parent:{{money}}<child :m.sync="money"/></div>',
        data:{
          money:100,
        },
        components:{
            child:{
                props:['m'],
                template:'<div>child:{{m}}<button @click="more">more</button></div>',
                methods:{
                    more(){
                        this.$emit('update:m',1000)
                    }
                }
            }
        }
    })
```


## 4、平级交互
- eventBus  但是不用  ->vuex
### 传递方式
A-B
- 给A绑定事件并增加方法`<div>{{val}} <button @click="changec">变c</button></div>`
- 给B订阅该方法`eventBus.$on('changed',(data)=>{this.val=data;})`
```
let eventBus=new Vue;
    //没人用，1、使用起来不好管理；2、命名冲突而且复杂  解决方式：vuex统一状态管理
    let c={
        created(){
            eventBus.$on('changed',(data)=>{this.val=data;})
        },
        template:'<div>{{val}} <button @click="changec">变c</button></div>',
        data(){return {val:'c'}},
        methods:{
            changec(){eventBus.$emit('changec','c')}
        }
    };
    let d={
        created(){
            eventBus.$on('changec',(data)=>{this.val=data})
        },
        template:'<div>{{val}}<button @click="changed">变d</button></div>',
        data(){return {val:'d'}},
        methods:{
            changed(){eventBus.$emit('changed','d')}
        }
    };

    let vm=new Vue({
        el:'#app',
        template:'<div><c></c><d></d></div>',
        components:{c,d}
    })
```

## 5、父组件调用子类方法
- ref=>this.$refs.xxx.子类方法
- 父类调用子组件的方法，子组件暴露一些方法，让父组件调用
- ref如果写在dom元素上，表示获取dom元素；如果写在组件上，表示当前组件的实例
- 父组件拿到子组件的方法后需要使用mounted，如果写在created上市拿不到子组件的方法，只有等待组件挂载完成后才能获取到。
```
let vm=new Vue({
    el:'#app',
    template:'<child ref="c"/>',
    mounted(){//如果写created是拿不到c的,只有等待组件挂载完成后才能获取
        this.$refs.c.fn()
    },
    components:{
        child:{
            template:'<div>child</div>',
            methods:{
                fn(){
                    alert(1)
                }
            }
        }
    }
})
```

## slot  插槽（设置一些空闲的位置等待使用）
- slot是vue提供的内置插件
- 具名slot：在写内容时，第一预留出来slot插口，如果没有使用，则使用默认值；
- 没有指定slot名字的都叫default，会塞到name=default的组件内，这个默认名可以不写

```
let vm = new Vue({
        el:'#app',
        components:{
            hello:{
                template:'#hello'
            }
        }
    })
    
    <div id="app">
        <hello>
            123
            <ul slot="bottom">
                <li>我是一号柚子</li>
                <li>我是二号柚子</li>
            </ul>
            <ul slot="top">
                <li>苹果</li>
            </ul>
            456
        </hello>
    </div>
    <template id="hello">
        <div>你好
            <slot name="default">nihao </slot>
            <slot name="top">这是上</slot>
            <slot name="bottom">这是下</slot>
        </div>
    </template>
    
页面渲染如下：
    你好 123 456
    苹果
    我是一号柚子
    我是二号柚子
```
## 弹框组件

```
<div id="app">
    <button @click="isShow=true">弹</button>
    <modal :show="isShow" @close="fn">
        <h3 slot="title">确定删除吗？</h3>
    </modal>
</div>
<template id="modal">
    <transition enter-active-class="animated fadeIn" leave-active-class="animated fadeOut">
        <div v-show="show">
            <div class="modal">
                <slot name="title">ok成功~~~</slot>
                <span class="close" @click="close">&times;</span>
            </div>
            <div id="mask" @click="close"></div>
        </div>
    </transition>
</template>

<script>
        let modal = {
        template: '#modal',
        props: {
            show: {//这里会将show挂载在当前实例上
                type: Boolean
            }
        },
        methods: {
            close() {
                this.$emit('close', false)
            }
        }
    };
    let vm = new Vue({
        el: '#app',
        data: {isShow: false,},
        components: {modal,},
        methods: {
            fn(result) {
                this.isShow = result;
            }
        }
    })
</script>
```

## 动画
1. transition可以用以下两个属性控制显示隐藏的动画
- enter-active-class
- leave-active-class

## 生命周期
```
let vm=new Vue({
        beforeCreated(){},
        le:'',//指定数据范围的元素
        data:{},//定义数据的地方，实例会代理这个数据
        methods:'',//一些事件和方法
        created(){},//事件和数据初始化后执行created方法
        template:'',//如果有模板，使用模板中的内容进行编译
        beforeMount(){},//挂载之前
        mounted(){},//用数据和视图渲染完成的事件
        computed:{},//计算属性
        beforeUpdate(){},//页面上的数据变化会触发update方法
        updated(){},//
        watch:{},//用来监控数据变化
        beforeDestroy(){},
        destroyed(){},
        directives:{},//自定义指令
        filters:{},//自定义过滤器
        a:'hi'//自定义属性
    });
    console.log(vm.$options.a)//可以获取自定义属性  vm.$options
```
