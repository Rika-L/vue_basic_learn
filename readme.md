# Vue学习

## 1-初识Vue
* el用于指定当前Vue实例为哪个容器服务，值通常为css选择器字符串

* 插值语法{{name}}

* data中用于存储数据，数据用于供el所指定的容器去使用，值暂时先写成一个对象

### 总结：

1. 想让Vue工作就必须创建一个Vue实例，并且传入一个配置对象
2. 容器里的代码依然符合html规范，只不过混入一些特殊的Vue语法
3. 容器内的代码被称为[Vue模板]
4. 容器和实例中的关系是一一对应
5. {{}}里面必须写js表达式
6. 一旦data中的数据发生改变, 那么模板中用到该数据的地方也会自动更新

注意区分 js表达式 和 js代码

1. 表达式:一个表达式会产生一个值, 可以放在任何一个需要值的地方
2. 语句:宏观

## 2-模板语法

### 插值语法

```html
{{xxx}}
```

在标签体当中通常用插值语法

### 指令语法

用于解析标签

在标签属性通常用指令语法

 ```html
 v-bind:  ===>  :
 ```

绑定

```html
<a v-bind:href="xxx"></a>
```

将引号里的东西当成表达式执行

也可以直接简写成:

```html
<a :href="xxx"></a>
```

xxx要写js表达式

## 3-Vue数据绑定

单向绑定数据

```html
v-bind:
```

双向绑定数据

```html
v-model:
```

* v-model:只能应用在表单类元素(输入类元素)上

简写:

```html
v-model:value  ===>  v-model
```

## 4-el与data的两种写法

### el

1. 直接写好el

```vue
new Vue({
        el:'#root',
        data:{
        
        }
    })
```

2. 最后再指定

```vue
const v = new Vue({
        data:{
        
        }
    })
v.$mount('#root'); //mount挂载
```

### data

1. 对象形式

```vue
new Vue({
        el:'#root',
        data:{
        	name: 'Rika',
        }
    })
```

2. 函数式

```vue
new Vue({
        el:'#root',
        data:function (){
            return{
				//此处的this是Vue对象
                name:'Rika',
            }
        }
    })
```

data函数不能写成箭头函数

```vue
new Vue({
        el:'#root',
        data(){
            return{
				//此处的this是Vue对象
                name:'Rika',
            }
        }
    })
```

以后学到组件,data必须用函数式

* 重要原则:由Vue管理的函数,一定不要写箭头函数,一旦写了箭头函数,this就不再是vue实例了

## 5-MVVM模型

```vue
const vm = new Vue({
		 el:'#root'
        data:{
        }
    })
```

1. M: 模型(model): data中的数据
2. V: 视图(view): 模板代码
3. VM: 视图模型(viewmodel): Vue实例



1. data中的所有属性,最终都出现在vm身上
2. vm身上的所有属性,以及vue原型上的所有属性,在vue模板中都可以直接使用

## 6-数据代理

### 1.回顾Object.defineproperty方法

没学过能叫回顾吗

```js
Object.defineproperty(对象, '属性', {配置项})
```
传入一个对象,遍历并生成数组

```js
Object.keys(对象)
```

Object.defineproperty方法加入的属性不可枚举

修改办法:在配置项中加入

```
enumerable: true,
```

控制属性是否可以枚举，默认值是false

```
writable: true,
```

控制属性是否可以被修改，默认值是false

```
configurable: true,
```

控制属性是否可以被删除，默认值是false

```
get:function(){
    return 'hello';
}
```

当有人读取person的age属性时，getter会被调用，返回值就是age的值

```
set(value){
    number=value;
}
```

当有人修改person的age属性时，setter就会被调用，且会收到修改的具体值

### 2.何为数据代理

通过一个对象代理另一个对象中属性的操作（读/写）

```js
let obj = {x:100};
let obj2 = {y:200}

Object.defineProperty(obj2, 'x', {
    get(){
        return obj.x;
    },
    set(value){
        obj.x = value;
    }
})
```

### 3.Vue中的数据代理

* 读取name， getter工作
* 修改name， setter工作

![04a9791cec0e00a27f0c839029a5baf](D:\wechatfile\WeChat Files\wxid_0z1kzagu3rpg22\FileStorage\Temp\04a9791cec0e00a27f0c839029a5baf.jpg)

data会保存在vm的属性_data中

```js
vm._data = options.data = data
```

所以验证:

```js
vm._data === data
```

![0cab40451165821adc1178d5b6ee4c3](D:\wechatfile\WeChat Files\wxid_0z1kzagu3rpg22\FileStorage\Temp\0cab40451165821adc1178d5b6ee4c3.jpg)

1. 通过vm对象来代理data对象中的属性操作
2. 更加方便操作data中的数据

## 7-事件处理

### 1.事件的基本使用

绑定事件

```html
v-on:click="函数名"
```

事件要写在配置项中

用methods

```vue
methods:{
            函数名(event){
                函数
            }
```

event事件出发的元素

```js
methods:{
            showInfo(event){
                console.log(event.target);
                console.log(this);
            }
        }
```

此处的this是vm对象

如果用箭头函数,this是Window

原则:

* 接受了Vue的管理,最好都写成普通函数,不要使用箭头函数

简写:

```js
@click="函数名"
```

```html
<button @click="showInfo">点我提示信息</button>
```

传参:

```html
<button @click="showInfo2(66, $event)">点我提示信息</button>
```

直接用括号传

$event占位符

2.事件修饰符

#### 1.阻止默认事件(常用)

```js
.prevent
```

```js
<a href="https://www.bilibili.com/" @click.prevent="showInfo">点我提示信息</a>
```

#### 2.阻止事件冒泡(常用)

```html
.stop
```

```html
<div @click="showInfo" class="demo1">
        <button @click.stop="showInfo">点我提示信息</button>
    </div>
```

#### 3.事件只会触发一次(常用)

```html
.once
```

```html
<button @click.once="showInfo">点我提示信息</button>
```

#### 4.capture使用事件的捕获模式

```html
.capture
```

先捕获在冒泡

#### 5.self

只有event.target是当前操作的元素时才触发事件

```html
.self
```

#### 6.passive

事件的默认行为立即执行,无需等待事件回调执行完毕

滚动事件:

wheel

scroll

### 2.键盘事件

keydown按下去就触发

keyup抬起才触发

```
<input type="text" placeholder="按下回车提示输入" @keyup.enter="showInfo">
```

```vue
.enter
```

别名

* 回车 enter
* 删除 delete (捕获删除和退格)
* 退出 esc
* 空格 space
* 换行 tab(特殊,必须配合keydown使用)
* 上 up
* 下 down
* 左 left
* 右 right

用法特殊:Ctrl, Alt, shift, meta **系统修饰键**

1. 配合keyup:按下修饰键的同时,再按下其他键,随后释放其他建,事件才会被触发
2. 配合keydown使用,正常触发事件

也可以用键码,不被推荐,已被弃用

自定义键名

```vue
Vue.config.keyCodes.huiche = 13//定义了一个别名按键
```

不太推荐

### 3.总结

* 修饰符可以连用,有先后顺序
* 键盘输入, 系统修饰键可以与普通键盘连用

## 8-计算属性

### 1.姓名案例_插值语法实现

### 2.姓名案例_methods实现

* 模板中的表达式要简单

methods方法:从this中找出参数

```vue
methods:{
    fullName(){
        return this.firstName+'-'+this.lastName;
    }
}
```

这么写效率不高

### 3.姓名案例_计算属性实现

#### get

```vue
computed:{
            fullName:{
                //当有人读取fullName时,get就会被调用,且返回值就作为fullName的值
                get(){

                }
            }
        }
```

也在vm上

get会有缓存

get什么时候调用:

1. 初次读取fullName时
2. 所依赖的数据发生变化时

与methods相比的优势:缓存(复用), 效率更高,调试更方便

#### set

用于修改

set什么时候调用:

1. 当fullName被修改时

```js
computed:{
    fullName:{
        //当有人读取fullName时,get就会被调用,且返回值就作为fullName的值
        get(){
            return this.firstName + '-' + this.lastName;
        },
        set(value){
            const arr = value.split('-');
            this.firstName = arr[0];
            this.lastName = arr[1];
        }
    }
}
```

以上为完整示例

### 4.计算属性简写

**更多的情况:只读取,不修改**

* 简写的前提:要确定只读不改才可以简写

```js
computed: {
    fullName() {
        return this.firstName + '-' + this.lastName;
    }
}
```

等于直接调用getter

### 5.总结

1. 计算属性最终会出现在vm上,直接读取使用即可
2. 如果计算属性要被修改,必须要写set函数,且set函数中必须要引起计算式以来的数据发生变化

## 9-监视属性

### 1.天气案例

绑定事件的时候

```vue
@xxx = 'yyy' 
```

yyy可以写一些简单的语句

### 2.天气案例_监视属性

```vue
immediate: true
```

watch

```vue
watch:{
    isHot:{
        //当isHot发生改变时被调用
        handler(newValue, oldValue){
            console.log(newValue, oldValue);
        },
        //immediate: true,//立即执行, 初始化时调用handler
    }
}
```

另一种方式:

最后再写watch

```vue
vm.$watch('isHot', {
    handler(newValue, oldValue) {
        console.log(newValue, oldValue);
    },
})
```

* 监视属性变化时,回调函数自动调用
* 监视属性必须存在,才能进行监视
* 监视属性的两种写法

### 3.深度监视

```vue
numbers: {
    deep:true,
    handler(newValue, oldValue) {
        console.log('number改变了');
    }
}
```

监视多级结构中所有属性的变化

* Vue中的watch默认不监测对象内部值的改变
* 配置deep:true,可以监测对象内部值的改变
* Vue自身可以监测对象内部值的变化
* 使用watch的时候根据数据的具体结构,决定是否采用深度监视

### 4.监视的简写

* 简写的前提:不需要immdiate,deep及其他配置项

```vue
watch: {
    
    //正常写法
    // isHot: {
    //     handler(newValue, oldValue) {
    //         console.log(newValue, oldValue);
    //     },
    // },
    
    //简写
    isHot(newValue, oldValue){
        console.log(newValue, oldValue);
    },
}
```

```vue
//正常写法
// vm.$watch('isHot', {
//     handler(newValue, oldValue) {
//         console.log(newValue, oldValue);
//     }
// })

//简写
vm.$watch('isHot', function(newValue, oldValue){
    console.log(newValue, oldValue);
})
```

不能写成箭头函数

### 5.computed对比watch

![image-20240126165355640](C:\Users\22093\AppData\Roaming\Typora\typora-user-images\image-20240126165355640.png)

**computed和watch之间的区别**

* computed能完成的,watch都可以完成
* wantch能完成的功能,computed不一定能完成.例如:watch能进行异步操作

**两个重要的yuanze**

* 所有被Vue管理的函数,最好写成普通函数,这样this指向的才是vm或组件实例对象
* 所有不被vue管理的函数(定时器的回调函数,ajax回调函数等)最好写成箭头函数,这样this指向的才是vm或组件实例对象

## 10-绑定样式

![image-20240126232134812](C:\Users\22093\AppData\Roaming\Typora\typora-user-images\image-20240126232134812.png)

绑定class样式--字符串写法

适用于:样式的类名不确定,需要动态指定

![image-20240126231843059](C:\Users\22093\AppData\Roaming\Typora\typora-user-images\image-20240126231843059.png)

绑定class样式--数组写法

适用于:要绑定的样式个数不确定,名字也不确定

![image-20240126231911299](C:\Users\22093\AppData\Roaming\Typora\typora-user-images\image-20240126231911299.png)

绑定class样式--对象写法

适用于:要绑定的样式个数确定,名字也确定,但是要动态决定用不用

![image-20240127104856527](C:\Users\22093\AppData\Roaming\Typora\typora-user-images\image-20240127104856527.png)

绑定style(对象方法)

![image-20240127105057488](C:\Users\22093\AppData\Roaming\Typora\typora-user-images\image-20240127105057488.png)

数组写法，比较少见

### 总结

![image-20240127105636906](C:\Users\22093\AppData\Roaming\Typora\typora-user-images\image-20240127105636906.png)

## 11-条件渲染

使用v-show做条件渲染

```html
<h2 v-show="false">Welcome to {{name}}</h2>
```

使用v-if做条件渲染

```html
<h2 v-if="false">Welcome to {{name}}</h2>
```

v-if直接把结构删除掉，比较狠

![image-20240127112103136](C:\Users\22093\AppData\Roaming\Typora\typora-user-images\image-20240127112103136.png)

如果第一个if执行后面直接跳过,类似于正常编程的流程

还有个v-else

* div紧密相连,不能被打断

![image-20240127115908499](C:\Users\22093\AppData\Roaming\Typora\typora-user-images\image-20240127115908499.png)

template不影响结构

只能配合v-if,不能使用v-show

### 总结

* v-if适合切换频率较低的场景,因为会将不展示的DOM元素直接移除
* v-show适用于切换频率较高的场景,仅仅只是将样式隐藏

## 12-列表渲染

### 1.基本列表

v-for循环遍历

```html
<li v-for="p in persons" :key="p.id">{{p.name}}-{{p.age}}</li>
```

注意加key

![image-20240127130801168](C:\Users\22093\AppData\Roaming\Typora\typora-user-images\image-20240127130801168.png)

```html
v-for="(p, index) in persons" :key="index"
```

```vue
persons: [
    {id: '001', name: '张三', age: 18},
    {id: '002', name: '李四', age: 19},
    {id: '003', name: '王五', age: 20},
],
```

遍历数组

接受多个参数:p是数据,key是索引

也可以用of

```html
<li v-for="(p, key) in car" :key="key">{{key}}-{{p}}</li>
```

```vue
car: {
    name: '奥迪a8',
    price: '70w',
    color: 'green',
}
```

遍历对象

```html
<li v-for="(char, index) in str" :key="index">{{char}}-{{index}}</li>
```

```
str:'hello',
```

遍历字符串,少见

```html
<li v-for="(number, index) in 5" :key="index">{{number}}-{{index}}</li>
```

遍历指定次数,用得更少

### 2. key的原理

![1290ce3858bb99d38370e31261945fe](D:\wechatfile\WeChat Files\wxid_0z1kzagu3rpg22\FileStorage\Temp\1290ce3858bb99d38370e31261945fe.jpg)

用index作为key会出问题：产生逆序添加/删除

会产生没有必要的真实DOM更新，效率低

![81f981eb199acc62ced9dfacd10c66d](D:\wechatfile\WeChat Files\wxid_0z1kzagu3rpg22\FileStorage\Temp\81f981eb199acc62ced9dfacd10c66d.jpg)

用id就不会出问题

如果你没写key，vue会自动将index补为key

如果不存在对数据的逆序添加/逆序删除等破坏顺序的操作，仅用于渲染列表用于展示，使用idnex作为key没问题

### 3.列表过滤

```vue
new Vue({
    el: '#root',
    data: {
        keyword: '',
        persons: [
            {id: '001', name: '马冬梅', age: 18, sex: '女'},
            {id: '002', name: '周冬雨', age: 19, sex: '女'},
            {id: '003', name: '周杰伦', age: 20, sex: '男'},
            {id: '004', name: '温兆伦', age: 21, sex: '男'},
        ],
        filPersons: [],
    },
    watch: {
        keyword: {
            immediate: true,
            handler(value) {
                this.filPersons = this.persons.filter((p) => {
                    return p.name.indexOf(value) !== -1;
                })
            }
        }
    }
})
```

用watch实现列表过滤

```vue
//计算属性实现
new Vue({
    el: '#root',
    data: {
        keyword: '',
        persons: [
            {id: '001', name: '马冬梅', age: 18, sex: '女'},
            {id: '002', name: '周冬雨', age: 19, sex: '女'},
            {id: '003', name: '周杰伦', age: 20, sex: '男'},
            {id: '004', name: '温兆伦', age: 21, sex: '男'},
        ],

    },
    computed: {
        filPersons() {
            return this.filPersons = this.persons.filter((p) => {
                return p.name.indexOf(this.keyword) !== -1;
            })
        }
    }
})
```

计算属性实现

### 4.列表排序

```vue
computed: {
    filPersons() {
        const arr = this.filPersons = this.persons.filter((p) => {
            return p.name.indexOf(this.keyword) !== -1;
        });
        //判断是否需要排序
        if (this.sortType) {
            arr.sort((a, b) => {
                return this.sortType === 1 ? b.age - a.age : a.age - b.age;
            })
        }
        return arr;
    }
}
```

优雅！

### 5.更新时的一个问题

```vue
methods: {
    updateMei() {
        // this.persons[0].name = '马老师';
        // this.persons[0].age = 50;
        // this.persons[0].sex = '男';
        // this.persons[0] = {id: '001', name: '马老师', age: 60, sex: '男'}//不奏效
    }
}
```

不奏效的行没有监测到信息改变了

修改方法:

```vue
this.persons.splice(0, 1,  {id: '001', name: '马老师', age: 60, sex: '男'})
```

### 6.7.Vue监测数据改变的原理_对象

```javascript
<script type="text/javascript" >

    let data = {
        name:'尚硅谷',
        address:'北京',
    }

    //创建一个监视的实例对象，用于监视data中属性的变化
    const obs = new Observer(data)
    console.log(obs)

    //准备一个vm实例对象
    let vm = {}
    vm._data = data = obs

    function Observer(obj){
        //汇总对象中所有的属性形成一个数组
        const keys = Object.keys(obj)
        //遍历
        keys.forEach((k)=>{
            Object.defineProperty(this,k,{
                get(){
                    return obj[k]
                },
                set(val){
                    console.log(`${k}被改了，我要去解析模板，生成虚拟DOM.....我要开始忙了`)
                    obj[k] = val
                }
            })
        })
    }
```

### 8.Vue.set()

```vue
Vue.set(vm.student, 'sex', '男')
```

```vue
vm.$set(vm.student, 'sex', '女')
```

作用一样

响应式

```vue
Vue.set(vm._data, 'leader', '一个帅气的男老师')
```

局限性:不能直接往vm对象上或者vm._data上设置

![image-20240127172202656](C:\Users\22093\AppData\Roaming\Typora\typora-user-images\image-20240127172202656.png)

### 9.Vue监测数据改变的原理_数组

Vue对数组的监测靠包装数组身上的常用修改数组的方法实现的

![image-20240127180343012](C:\Users\22093\AppData\Roaming\Typora\typora-user-images\image-20240127180343012.png)

也可以这样改

```vue
Vue.set(vm.student.hobby,0,'打台球')
```

### 10.总结Vue数据监测

* Vue会监视data中所有层次的数据
* 通过setter实现监视，且要在new Vue时就传入要监测的数据
* 对象后追加的属性，Vue默认不做响应式处理
* 如需给后添加的属性做响应式，使用如下API
* 1. Vue.set(target， propertyName/index， value)
  2. vm.$set(target， propertyName/index， value)
* 如何监测数组中的数据:
* 1. 包裹数组更新元素的方法实现
* 在Vue中修改数组中的某一个元素一定要用如下方法:
* 1. push()
  2. pop()
  3. shift()
  4. unshift()
  5. splice()
  6. sort()
  7. reverse()
* 1. Vue.set()
  2. vm.$set()
* Vue.set() 和 vm.$set() 不能给 vm 或 vm 的根数据对象 添加属性!!!![image-20240127191253432](C:\Users\22093\AppData\Roaming\Typora\typora-user-images\image-20240127191253432.png)

## 13-收集表单数据

修饰符：

* .number 收集成数字类型
* .lazy 防抖,输入完失去焦点才会收集
* .trim 去掉前后空格

若

```html
男 <input type="radio" name="sex" value="man" v-model="userInfo.sex">
女 <input type="radio" name="sex" value="woman" v-model="userInfo.sex"><br/><br/>
```

v-model收集的是value值,且要给标签配置value

若

```html
爱好：
学习<input type="checkbox" v-model="userInfo.hobby" value="study">
打游戏<input type="checkbox" v-model="userInfo.hobby" value="game">
吃饭<input type="checkbox" v-model="userInfo.hobby" value="eat"><br/><br/>
```

1. 没有配置value属性,那么手机的就是checked(勾选或者未勾选,是布尔值)
2. 配置input的value属性
3. * v-model的初始值是非数组,收集的是checked
   * v-model初始值是数组,收集的就是value组成的数组

## 14-过滤器

```html
<!--    过滤器实现-->
<h3>现在是{{time | timeFormater}}</h3>
```

```vue
filters: {
    timeFormater(value) {
        return dayjs(value).format('YYYY-MM-DD HH:mm:ss')
    }
},
```

多个过滤器之前可以串联

全局过滤器

```vue
Vue.filter('mySlice', function (value) {
    return value.slice(0, 4);
})
```

Vue3已经移除了过滤器

适用于一些简单逻辑的判断

## 15-内置指令

### 1.v-text

```html
<div v-text="xxx"></div>
```

会拿到xxx的值替换整个div的内容， 里面的内容不能解析标签

### 2.v-html

作用跟v-text相同，但是能解析HTML

![8e8d3017b47e0f864b48a3df5976f31](D:\wechatfile\WeChat Files\wxid_0z1kzagu3rpg22\FileStorage\Temp\8e8d3017b47e0f864b48a3df5976f31.jpg)

有安全隐患

### 3.v-cloak

```
v-cloak
```

本质是一个特殊属性，Vue实例创建完毕并接管容器后，会删掉v-cloak属性

使用css配合可以解决网速慢时页面展示出{{}}的问题

```css
<style>
    [v-cloak] {
        display: none;
    }
</style>
```

### 4.v-once

```vue
v-once
```

v-once所在的节点在初次动态渲染后,就视为静态内容了

以后数据变化不会引起v-once所在结构的更新,用于优化性能

### 5.v-pre

```vue
v-pre
```

 跳过其所在节点的编译过程

## 16-自定义指令

### 1.函数式

自定义指令何时会被调用：

1. 指令与元素成功绑定时
2. 指令所在的模板重新解析时

```html
<h2>放大十倍后的n值是：<span v-big="n"></span></h2>
```

```vue
directives: {
    big(element, binding) {
        element.innerHTML = binding.value * 10;
    }
}
```
