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

