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
