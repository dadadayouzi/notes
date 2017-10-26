##VUE基础
### 1.循环
#### 1.1声明式：不需要关心如何实现
- forEach
没有返回值，不能跳出循环（for循环可以跳出循环）

- map——映射
将一个数组映射成一个新数组，返回什么就会把当前返回的放到一个新数组中
```
let newArr = ['1','2','3'].map((item,index)=>`<li>${item}<li>`);
```
- filter——过滤
返回值是一个过滤后的数组，如果回调函数中返回true，表示放到新数组中，返回false表示过滤掉
```
let newArr = [1,2,3,4,5].filter((item,index)=>item!==3);
console.log(newArr);//[ 1, 2, 4, 5 ]
```
 - find——查找
 - 找到的是住宿中某一项，如果找不到返回undefined
 - 找到一次后就会停止
```
let item = [1,2,3].find(item=>item===3);
console.log(item);//3
```
 - includes / some / every
 - 判断数组中有没有
 - 返回boolean类型
 - includes和some找true
 - every找false，找到后就返回不再继续查找
```
console.log([1,2,3].includes(3));//false
console.log([1,2,3].some(item=>item>3));//找true   false
console.log([1,4,3].every(item=>item>3));//找false  false
```
- reduce——收敛
 - 返回一个叠加后的结果
 - 返回结果会作为下一次的上一个
 - 有四个参数：prev、next、index、oldArray
```

```
#### 1.2编程式
- for
- for in
- for of

### 2.npm安装
- 全局安装：在命令行里使用的才是全局安装
- 本地安装

### 3.VUE
#### 3.1 VUE数据驱动，渐进式
- 以前操作dom，现在操作的是数据，以数据为核心，数据的结构对应视图
- 借鉴了angular的一些“指令” 扩展一些html标签,就是html行间的属性，而且这个属性给标签扩展了一些功能。
- 借鉴了react的组件系统，将一个很大页面拆分成若干个组件，可以复用，而且方便了代码的维护。
- 路由管理，访问不同的路径可以返回不同的结果，前端路由 spa 单页应用，就一个html 根据不同的路径切换里面组件 vue-router
- 状态管理 组件之间的数据通信 vuex
- 可以提供一个生成项目工具 vue-cli

### 3.2 数据驱动视图—视图可以改变数据（双向绑定）
- **MVC**
> controller -> model -> view

- **MVVM**
> MVVM模式只针对于可以编辑可改变的元素：input  tectarea（表单类型的）


### 3.3 this问题：如何改变this指向
- **let that=this**
- **bind**
> bind只能绑定一次，多次绑定无效

- **箭头函数**
> 特点
> - 箭头函数中没有this指向，自己家找不到就会去上一级找，从而解决了this问题
> - 没有arguments

**PS：高阶函数**（函数返回函数：箭头函数形式）
```
let total= a=>b=>a+b;
```


### 3.4 vue常用指令
- **v-model**   实现双向绑定
- **v-once**     绑定一次
- **v-html**  转译html
- **v-text**  解决局部闪烁问题
- **v-cloak** 解决闪烁问题
- **v-for** 循环：数组、对象，将数据变为dom结构
- **v-if  /  v-show**  控制显示和隐藏


### 3.5 vue常用指令的使用
####注意：
> 1、属性如果没有声明，则不能取值；
> 2、{{a:a}}如果对象中没有此属性，则不支持数据绑定，数据改变视图也不会改变
> 3、可以用新的属性替换掉老的，可以实现后添加属性，$set也可以实现后添加属性

- **v-model**
```
let vm = new Vue({
        el:'#app',
        data:{
            msg:'<h1>hello</h1>'
        }
    });
input中使用：
它会先将数据绑定到输入框中，当输入框中的内容变化会影响数据的变化，视图会再次刷新
<input type="text" v-model="msg">

```

- **v-once**
```
v-once里面的内容 不会再次监控数据的变化
输入框中内容变化，下面这行代码在页面不会有任何变化
<div v-once>{{msg}}</div>
```

- **v-html**
```
v-html 可以将内容转化成html标签插入到div中
这行代码可以将msg中的内容正确转化为html格式
<div v-html="msg"></div>
```

- **v-text**
```
v-text和{{}}是等价的 属性时不会被用户看到的，编译好后再插入到div中，**防止闪烁问题**
<div v-text="msg"></div>
```
- **v-for**
```
循环数据：将它放到需要循环的元素上
<div v-for="product in products"></div>

循环的是数组：
如果一个值 就是value of数组，两个就是（value，index） of/in
<ul>
    <li v-for="(fruit,index) in fruits">{{index}}{{fruit}}</li>
</ul>

循环的是对象：
key 指的是对象中的键 不在是index索引了
<ul>
    <li v-for="(val,key) in school">{{val}} {{key}}</li>
</ul>
```

### 4. 绑定事件

- 原生js在行内绑定事件需要加括号，需要事件源可以传递event
- vue中事件可以使用**@替换v-on**
 - 所有**this指向**的都是当前实例
 - 如果传递参数需要事件源可以传入**$event**
 - data中的名字和methods中不能相同

### 5. 显示隐藏
##### v-show  /  v-if
> - v-show  控制的是样式
> - v-if  控制的结构
> - 如果频繁切换显示隐藏最好用v-show

### 6. 修饰符
事件的行为默认是冒泡，从内往外
- .stop  阻止冒泡
- .capture 表示事件捕获 先捕获 -> 在到目标 -> 在到冒泡
- .self 表示只在自己身上触发
- .prevent 阻止默认事件
- .键盘事件  修饰符可以连续调用

### 7. 变量
####7.1 v-bind
- 可以简写成：形式
- 注意：{{}}是取值表达式，一般只用在html标签中，如果用在属性中，会报错
```
<div :title="msg">hello</div>

//以下代码会报错
<div title="{{msg}}">hello</div>
```

#### 7.2 class 和 style
- 可以动态绑定，写法特殊：可以使用对象和数组
 - 与原生的class不冲突；动态和静态生效的是根据前后顺序，看的是权重；
 - 数组中表示两个都生效，如果是类名，需要用‘’包起来，也可以使用变量，不用加‘’
```
<style>
  .red{color: red;}
  .back1{background: yellow;}
  .back2{background: red;}
</style>

<div :class="red">vue</div>//错误写法
<div :class="{back2:flag}" class="back1">vue</div>
<div :class="['back1','back2']">呵呵</div>

<div :style="{fontSize:'40px',background:'red'}">111</div>
<div :style="[{color:'red'},{background:'yellow'}]">222</div>
```

### 8.  **created**
> 当实例创建后就会执行该方法
> - 只提供方法
> - ajax请求数据放在里面（使用axios获取数据）
>   - axios获取数据，默认不会挂载在实例上
>   - axios好处：这个库前后端通用
> ```
axios.get('./product.json').then((res)=>{
      this.products=res.data;//data就是最后的结果
 },(err)=>{console.log(err)})
> ```
> - 触发该事件后，vue的this指向当前实例

### 9. **computed**
不支持异步
> - 看起来是函数，其实是属性
> - 可以放对象，可以放方法
> - 可以计算、获取
>   - 根据别人的值计算而来
>   - 当给这个值赋值可能影响其他人
>   

### 10. watch
支持异步——因为没有返回值
> - 监控、观察，值改变就做一些事，目前用来本地存储

### 11. 如何获取元素
```
<div ref="ok">hello</div>
//ref的作用是给一个元素添加一个别名，可以在this.$refs上拿到这个元素
```

### 12. 生命周期
- 组件的生命周期，根实例可以看成一个组件
- template 只能有一个根元素，而且不能是文本
- 常用的：
 - created  
 - mounted  
 - beforeDestroy
```
//创建之前：一般用不到——因为数据没有被挂载，方法也没有挂载
beforeCreate(){
      alert('beforeCreate')
},

//发送ajax请求，请求是异步的，会直接走到beforeMount中
created(){
      alert('created')
},

//判断有没有el属性，看有没有template,有就用template，没有就用id=app内部元素渲染；
beforeMount(){
      alert('beforeMount')
},

//获取真实dom元素  this.$refs.ok
mounted(){
      alert('mounted')           
},

//不会触发，当页面上的数据发生变化时会执行此方法，前提是页面用到了这个数据（一般会用watch校验数据变化）
beforeUpdate(){
      alert('beforeUpdate')
},

//不会触发，
updated(){
      alert('updated')
},

beforeDestroy(){
      alert('beforeDestroy')
},
        
destroyed(){
      alert('destroyed')
}
```
**手动调用**
```
vm.$mount('#app');//挂载元素  手动挂载
vm.$destroy();//go die :一般不会手动调用
```

### 13. promise——异步——ES6
- 通过回调函数可以获得异步编程的值，但是有缺点：需要手动传入回调，promise可以解决这个问题

**回调函数**
```
function read(cb) {
    setTimeout(function () {
        let str='hi';
        cb(str)
    })
}
read(function (str) {
    console.log(str);
});
```

**promise固定模式**
- executor   执行器
- resolve，reject是函数类型
- resolve是then中第一个函数
- reject是then中第二个函数
```
function read() {
    return new Promise(function (resolve,reject) {
        setTimeout(function () {
            let str='hi';
            resolve(str);//1 'hi'(成功后就不会再失败)
            reject(str);
        },3000)
    });
}
//promise的一个实例
read().then(function (data) {
    console.log(1, data);
},function (data) {
    console.log(data);
});
```
