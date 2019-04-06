# Vue.js - Day1

## 课程介绍
前5天： 都在学习Vue基本的语法和概念；打包工具 Webpack , Gulp
后5天： 以项目驱动教学；


### 什么是Vue.js

+ Vue.js 是目前最火的一个前端框架，React是最流行的一个前端框架（React除了开发网站，还可以开发手机App， Vue语法也是可以用于进行手机App开发的，需要借助于Weex）

+ Vue.js 是前端的**主流框架之一**，和Angular.js、React.js 一起，并成为前端三大主流框架！

+ Vue.js 是一套构建用户界面的框架，**只关注视图层**，它不仅易于上手，还便于与第三方库或既有项目整合。（Vue有配套的第三方类库，可以整合起来做大型项目的开发）

+ 前端的主要工作？主要负责MVC中的V这一层；主要工作就是和界面打交道，来制作前端页面效果；





## 为什么要学习流行框架
 + 企业为了提高开发效率：在企业中，时间就是效率，效率就是金钱；
  - 企业中，使用框架，能够提高开发的效率；



 + 提高开发效率的发展历程：原生JS -> Jquery之类的类库 -> 前端模板引擎 -> Angular.js / Vue.js（能够帮助我们减少不必要的DOM操作；提高渲染效率；双向数据绑定的概念【通过框架提供的指令，我们前端程序员只需要关心数据的业务逻辑，不再关心DOM是如何渲染的了】）
 + 在Vue中，一个核心的概念，就是让用户不再操作DOM元素，解放了用户的双手，让程序员可以更多的时间去关注业务逻辑；



 + 增强自己就业时候的竞争力
  - 人无我有，人有我优
  - 你平时不忙的时候，都在干嘛？

## 框架和库的区别



 + 框架：是一套完整的解决方案；对项目的侵入性较大，项目如果需要更换框架，则需要重新架构整个项目。

  - node 中的 express；



 + 库（插件）：提供某一个小功能，对项目的侵入性较小，如果某个库无法完成某些需求，可以很容易切换到其它库实现需求。
  - 1. 从Jquery 切换到 Zepto
  - 2. 从 EJS 切换到 art-template







## Node（后端）中的 MVC 与 前端中的 MVVM 之间的区别

 + MVC 是后端的分层开发概念；
 + MVVM是前端视图层的概念，主要关注于 视图层分离，也就是说：MVVM把前端的视图层，分为了 三部分 Model, View , VM ViewModel

 + 为什么有了MVC还要有MVVM



## Vue.js 基本代码 和 MVVM 之间的对应关系

## Vue之 - `基本的代码结构`和`插值表达式`、`v-cloak`

```javascript
<script>
[v-cloak]{display:none}    
</script>
<!-- 默认 v-text 是没有闪烁问题的 -->
<!-- v-text会覆盖元素中原本的内容，但是 插值表达式  只会替换自己的这个占位符，不会把 整个元素的内容清空 -->
```



## Vue指令之`v-text`和`v-html`

v-text 可以解决网速慢的时候闪烁的问题

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <style>
    [v-cloak] {
      /* display: none; */
    }
  </style>
</head>
<body>
  <div id="app">
    <!-- 使用 v-cloak 能够解决 插值表达式闪烁的问题 -->
    <p v-cloak>++++++++ {{ msg }} ----------</p>
    <h4 v-text="msg">==================</h4>
    <!-- 默认 v-text 是没有闪烁问题的 -->
    <!-- v-text会覆盖元素中原本的内容，但是 插值表达式  只会替换自己的这个占位符，不会把 整个元素的内容清空 -->

    <div>{{msg2}}</div>
    <div v-text="msg2"></div>
    <div v-html="msg2">1212112</div>
    <!-- v-bind: 是 Vue中，提供的用于绑定属性的指令 -->
    <!-- <input type="button" value="按钮" v-bind:title="mytitle + '123'"> -->
      <!--上边的可以简写为下边这句，将v-bind去掉-->
    <input type="button" value="按钮" :title="mytitle + '123'">
    <!-- 注意： v-bind: 指令可以被简写为 :要绑定的属性 -->
    <!-- v-bind 中，可以写合法的JS表达式 -->
    <!-- Vue 中提供了 v-on: 事件绑定机制 -->
    <!-- <input type="jbutton" value="按钮" :title="mytitle + '123'" v-on:click="alert('hello')"> --><!--上边这样是不行的，因为veu会把alert('hello')当做一个变量去data中寻找，找不到会报错-->
    <input type="button" value="按钮" @click="show">
  </div>
  <script src="./lib/vue-2.4.0.js"></script>
  <script>
    var vm = new Vue({
      el: '#app',
      data: {
        msg: '123',
        msg2: '<h1>哈哈，我是一个大大的H1， 我大，我骄傲</h1>',
        mytitle: '这是一个自己定义的title'
      },
      methods: { // 这个 methods属性中定义了当前Vue实例所有可用的方法
        show: function () {
          alert('Hello')
        }
      }
    })
    /* document.getElementById('btn').onclick = function(){
      alert('Hello')
    } */
  </script>
</body>
</html>
<!-- 1. 如何定义一个基本的Vue代码结构 -->
<!-- 2. 插值表达式 和  v-text   -->
<!-- 3. v-cloak -->
<!-- 4. v-html -->
<!-- 5. v-bind   Vue提供的属性绑定机制   缩写是 : -->
<!-- 6. v-on     Vue提供的事件绑定机制   缩写是 @ -->
```


## Vue指令之`v-bind`的三种用法

1. 直接使用指令`v-bind`

2. 使用简化指令`:`

3. 在绑定的时候，拼接绑定内容：`:title="btnTitle + ', 这是追加的内容'"`


## Vue指令之`v-on`和`跑马灯效果`



### 跑马灯效果

1. HTML结构：

```html
<div id="app">
    <p>{{info}}</p>
    <input type="button" value="开启" v-on:click="go">
    <input type="button" value="停止" v-on:click="stop">
  </div>
```

2. Vue实例：

```javascript
	// 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {
        info: '猥琐发育，别浪~！',
        intervalId: null
      },
      methods: {
          // var _this=this  // 如果要不使用=>的话，就可以直接写一个函数，将_this直接可以在函数中使用，但是一般不这样使用
          /*setInterval(function(){
                var start=_this.info.substring(1);
              	var end=_this.info.substring(0, 1);
              	_this.info=start+end
            })*/
        go() {
          // 如果当前有定时器在运行，则直接return
          if (this.intervalId != null) {
            return;
          }
          // 开始定时器
            
            
          this.intervalId = setInterval(() => {  // =>代表可以把后边括弧中的方法中的this可以指向外边的this,这样的话可以直接引用this这个变量
            this.info = this.info.substring(1) + this.info.substring(0, 1);
          }, 500);
        },
        stop() {
          clearInterval(this.intervalId);
        }
      }
    });
```







## Vue指令之`v-on的缩写`和`事件修饰符`



### 事件修饰符：

+ .stop       阻止冒泡

+ .prevent    阻止默认事件

+ .capture    添加事件侦听器时使用事件捕获模式

+ .self       只当事件在该元素本身（比如不是子元素）触发时触发回调

+ .once       事件只触发一次


```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <script src="./lib/vue-2.4.0.js"></script>
  <style>
    .inner {
      height: 150px;
      background-color: darkcyan;
    }

    .outer {
      padding: 40px;
      background-color: red;
    }
  </style>
</head>

<body>
  <div id="app">
<!--下边的注释可以一个一个放开使用，观察效果-->
    <!-- 使用  .stop  阻止冒泡 -->
    <!-- <div class="inner" @click="div1Handler">
      <input type="button" value="戳他" @click.stop="btnHandler">
    </div> -->

    <!-- 使用 .prevent 阻止默认行为 -->
    <!-- <a href="http://www.baidu.com" @click.prevent="linkClick">有问题，先去百度</a> -->

    <!-- 使用  .capture 实现捕获触发事件的机制 -->
    <!-- <div class="inner" @click.capture="div1Handler">
      <input type="button" value="戳他" @click="btnHandler">
    </div> -->

    <!-- 使用 .self 实现只有点击当前元素时候，才会触发事件处理函数 -->
    <!-- <div class="inner" @click="div1Handler">
      <input type="button" value="戳他" @click="btnHandler">
    </div> -->

    <!-- 使用 .once 只触发一次事件处理函数 -->
    <!-- <a href="http://www.baidu.com" @click.prevent.once="linkClick">有问题，先去百度</a> -->


    <!-- 演示： .stop 和 .self 的区别 -->
    <!-- <div class="outer" @click="div2Handler">
      <div class="inner" @click="div1Handler">
        <input type="button" value="戳他" @click.stop="btnHandler">
      </div>
    </div> -->

    <!-- .self 只会阻止自己身上冒泡行为的触发，并不会真正阻止 冒泡的行为 -->
    <!-- <div class="outer" @click="div2Handler">
      <div class="inner" @click.self="div1Handler">
        <input type="button" value="戳他" @click="btnHandler">
      </div>
    </div> -->

  </div>

  <script>
    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {},
      methods: {
        div1Handler() {
          console.log('这是触发了 inner div 的点击事件')
        },
        btnHandler() {
          console.log('这是触发了 btn 按钮 的点击事件')
        },
        linkClick() {
          console.log('触发了连接的点击事件')
        },
        div2Handler() {
          console.log('这是触发了 outer div 的点击事件')
        }
      }
    });
  </script>
</body>
</html>
```

## Vue指令之`v-model`和`双向数据绑定`

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <script src="./lib/vue-2.4.0.js"></script>
</head>
<body>
  <div id="app">
    <h4>{{ msg }}</h4>
    <!-- v-bind 只能实现数据的单向绑定，从 M 自动绑定到 V， 无法实现数据的双向绑定  -->
    <!-- <input type="text" v-bind:value="msg" style="width:100%;"> -->
    <!-- 使用  v-model 指令，可以实现 表单元素和 Model 中数据的双向数据绑定 -->
    <!-- 注意： v-model 只能运用在 表单元素中 -->
    <!-- input(radio, text, address, email....)   select    checkbox   textarea   -->
    <input type="text" style="width:100%;" v-model="msg">
  </div>
  <script>
    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {
        msg: '大家都是好学生，爱敲代码，爱学习，爱思考，简直是完美，没瑕疵！'
      },
      methods: {
      }
    });
  </script>
</body>
</html>
```



## 简易计算器案例

1. HTML 代码结构

```
  <div id="app">
    <input type="text" v-model="n1">
    <select v-model="opt">
      <option value="0">+</option>
      <option value="1">-</option>
      <option value="2">*</option>
      <option value="3">÷</option>
    </select>
    <input type="text" v-model="n2">
    <input type="button" value="=" v-on:click="getResult">
    <input type="text" v-model="result">
  </div>
```

2. Vue实例代码：

```
	// 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {
        n1: 0,
        n2: 0,
        result: 0,
        opt: '0'
      },
      methods: {
        getResult() {
          switch (this.opt) {
            case '0':
              this.result = parseInt(this.n1) + parseInt(this.n2);
              break;
            case '1':
              this.result = parseInt(this.n1) - parseInt(this.n2);
              break;
            case '2':
              this.result = parseInt(this.n1) * parseInt(this.n2);
              break;
            case '3':
              this.result = parseInt(this.n1) / parseInt(this.n2);
              break;
          }
        }
      }
    });
```

## 在Vue中使用样式

### 使用class样式

1. 数组
```
<h1 :class="['red', 'thin']">这是一个邪恶的H1</h1>
```

2. 数组中使用三元表达式
```
<h1 :class="['red', 'thin', isactive?'active':'']">这是一个邪恶的H1</h1>
```

3. 数组中嵌套对象
```
<h1 :class="['red', 'thin', {'active': isactive}]">这是一个邪恶的H1</h1>
```

4. 直接使用对象
```
<h1 :class="{red:true, italic:true, active:true, thin:true}">这是一个邪恶的H1</h1>
```

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <script src="./lib/vue-2.4.0.js"></script>
  <style>
    .red {
      color: red;
    }
    .thin {
      font-weight: 200;
    }
    .italic {
      font-style: italic;
    }
    .active {
      letter-spacing: 0.5em;
    }
  </style>
</head>

<body>
  <div id="app">
    <!-- <h1 class="red thin">这是一个很大很大的H1，大到你无法想象！！！</h1> -->

    <!-- 第一种使用方式，直接传递一个数组，注意： 这里的 class 需要使用  v-bind 做数据绑定 -->
    <!-- <h1 :class="['thin', 'italic']">这是一个很大很大的H1，大到你无法想象！！！</h1> -->

    <!-- 在数组中使用三元表达式 -->
    <!-- <h1 :class="['thin', 'italic', flag?'active':'']">这是一个很大很大的H1，大到你无法想象！！！</h1> -->

    <!-- 在数组中使用 对象来代替三元表达式，提高代码的可读性 -->
    <!-- <h1 :class="['thin', 'italic', {'active':flag} ]">这是一个很大很大的H1，大到你无法想象！！！</h1> -->

    <!-- 在为 class 使用 v-bind 绑定 对象的时候，对象的属性是类名，由于 对象的属性可带引号，也可不带引号，所以 这里我没写引号；  属性的值 是一个标识符 -->
    <h1 :class="classObj">这是一个很大很大的H1，大到你无法想象！！！</h1>

  </div>
  <script>
    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {
        flag: true,
        classObj: { red: true, thin: true, italic: false, active: false }
      },
      methods: {}
    });
  </script>
</body>
</html>
```



### 使用内联样式

1. 直接在元素上通过 `:style` 的形式，书写样式对象
```
<h1 :style="{color: 'red', 'font-size': '40px'}">这是一个善良的H1</h1>
```

2. 将样式对象，定义到 `data` 中，并直接引用到 `:style` 中
 + 在data上定义样式：
```
data: {
        h1StyleObj: { color: 'red', 'font-size': '40px', 'font-weight': '200' }
}
```
 + 在元素中，通过属性绑定的形式，将样式对象应用到元素中：
```
<h1 :style="h1StyleObj">这是一个善良的H1</h1>
```

3. 在 `:style` 中通过数组，引用多个 `data` 上的样式对象
 + 在data上定义样式：
```
data: {
        h1StyleObj: { color: 'red', 'font-size': '40px', 'font-weight': '200' },
        h1StyleObj2: { fontStyle: 'italic' }
}
```
 + 在元素中，通过属性绑定的形式，将样式对象应用到元素中：
```
<h1 :style="[h1StyleObj, h1StyleObj2]">这是一个善良的H1</h1>
```

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <script src="./lib/vue-2.4.0.js"></script>
</head>

<body>
  <div id="app">
    <!-- 对象就是无序键值对的集合 -->
    <!-- <h1 :style="styleObj1">这是一个h1</h1> -->

    <h1 :style="[ styleObj1, styleObj2 ]">这是一个h1</h1>
  </div>

  <script>
    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {
        styleObj1: { color: 'red', 'font-weight': 200 },
        styleObj2: { 'font-style': 'italic' }
      },
      methods: {}
    });
  </script>
</body>

</html>
```



## Vue指令之`v-for`和`key`属性

1. 迭代数组

```html
<ul>
  <li v-for="(item, i) in list">索引：{{i}} --- 姓名：{{item.name}} --- 年龄：{{item.age}}</li>
</ul><!-- 后边的 i 是遍历的下标-->
<script>
    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {
        list: [1, 2, 3, 4, 5, 6]
      },
      methods: {}
    });
  </script>
```

2. 迭代对象中的属性

```html
	<!-- 循环遍历对象身上的属性 -->
    <div v-for="(val, key, i) in userInfo">{{val}} --- {{key}} --- {{i}}</div>
<script>
    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {
        userInfo: {
          id: 1,
          name: '托尼·屎大颗',
          gender: '男'
        }
      },
      methods: {}
    });
  </script>
```

3. 迭代数字

```html

<p v-for="i in 10">这是第 {{i}} 个P标签</p>
<script>
    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {},
      methods: {}
    });
  </script>
```

4.迭代对象数组

```html
<body>
  <div id="app">
    <p v-for="(user, i) in list">Id：{{ user.id }} --- 名字：{{ user.name }} --- 索引：{{i}}</p>
  </div>

  <script>
    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {
        list: [
          { id: 1, name: 'zs1' },
          { id: 2, name: 'zs2' },
          { id: 3, name: 'zs3' },
          { id: 4, name: 'zs4' }
        ]
      },
      methods: {}
    });
  </script>
</body>
```



> 2.2.0+ 的版本里，**当在组件中使用** v-for 时，key 现在是必须的。



当 Vue.js 用 v-for 正在更新已渲染过的元素列表时，它默认用 “**就地复用**” 策略。如果数据项的顺序被改变，Vue将**不是移动 DOM 元素来匹配数据项的顺序**， 而是**简单复用此处每个元素**，并且确保它在特定索引下显示已被渲染过的每个元素。

```html
<body>
  <div id="app">

    <div>
      <label>Id:
        <input type="text" v-model="id">
      </label>

      <label>Name:
        <input type="text" v-model="name">
      </label>

      <input type="button" value="添加" @click="add">
    </div>

    <!-- 注意： v-for 循环的时候，key 属性只能使用 number获取string -->
    <!-- 注意： key 在使用的时候，必须使用 v-bind 属性绑定的形式，指定 key 的值 -->
    <!-- 在组件中，使用v-for循环的时候，或者在一些特殊情况中，如果 v-for 有问题，必须 在使用 v-for 的同时，指定 唯一的 字符串/数字 类型 :key 值 -->
    <p v-for="item in list" :key="item.id">
      <input type="checkbox">{{item.id}} --- {{item.name}}
    </p>
  </div>

  <script>
    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {
        id: '',
        name: '',
        list: [
          { id: 1, name: '李斯' },
          { id: 2, name: '嬴政' },
          { id: 3, name: '赵高' },
          { id: 4, name: '韩非' },
          { id: 5, name: '荀子' }
        ]
      },
      methods: {
        add() { // 添加方法
          //  this.list.push({id:this.id,name:this.name})  //  可以用push在后边插入，
          this.list.unshift({ id: this.id, name: this.name })  //  可以用unshift在前边插入
        }
      }
    });
  </script>
</body>
```

为了给 Vue 一个提示，**以便它能跟踪每个节点的身份，从而重用和重新排序现有元素**，你需要为每项提供一个唯一 key 属性。

## Vue指令之`v-if`和`v-show`

```html
<body>
  <div id="app">

    <!-- <input type="button" value="toggle" @click="toggle"> -->
    <input type="button" value="toggle" @click="flag=!flag">

    <!-- v-if 的特点：每次都会重新删除或创建元素 -->
    <!-- v-show 的特点： 每次不会重新进行DOM的删除和创建操作，只是切换了元素的 display:none 样式 -->

    <!-- v-if 有较高的切换性能消耗 -->
    <!-- v-show 有较高的初始渲染消耗 -->

    <!-- 如果元素涉及到频繁的切换，最好不要使用 v-if, 而是推荐使用 v-show -->
    <!-- 如果元素可能永远也不会被显示出来被用户看到，则推荐使用 v-if -->
    <h3 v-if="flag">这是用v-if控制的元素</h3>
    <h3 v-show="flag">这是用v-show控制的元素</h3>

  </div>

  <script>
    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {
        flag: false
      },
      methods: {
        /* toggle() {
          this.flag = !this.flag
        } */
      }
    });
  </script>
</body>
```

> 一般来说，v-if 有更高的切换消耗而 v-show 有更高的初始渲染消耗。因此，如果需要频繁切换 v-show 较好，如果在运行时条件不大可能改变 v-if 较好。

## 总结



```html
<!-- 1. MVC 和 MVVM 的区别 -->
<!-- 2. 学习了Vue中最基本代码的结构 -->
<!-- 3. 插值表达式   v-cloak   v-text   v-html   v-bind（缩写是:）   v-on（缩写是@）   v-model   v-for   v-if     v-show -->
<!-- 4. 事件修饰符  ：  .stop   .prevent   .capture   .self     .once -->
<!-- 5. el  指定要控制的区域    data 是个对象，指定了控制的区域内要用到的数据    methods 虽然带个s后缀，但是是个对象，这里可以自定义了方法 -->
<!-- 6. 在 VM 实例中，如果要访问 data 上的数据，或者要访问 methods 中的方法， 必须带 this -->
<!-- 7. 在 v-for 要会使用 key 属性 （只接受 string / number） -->
<!-- 8. v-model 只能应用于表单元素 -->
<!-- 9. 在vue中绑定样式两种方式  v-bind:class   v-bind:style --
```

## 品牌管理案例



### 添加新品牌



### 删除品牌



### 根据条件筛选品牌

1. 1.x 版本中的filterBy指令，在2.x中已经被废除：

[filterBy - 指令](https://v1-cn.vuejs.org/api/#filterBy)

```

<tr v-for="item in list | filterBy searchName in 'name'">

  <td>{{item.id}}</td>

  <td>{{item.name}}</td>

  <td>{{item.ctime}}</td>

  <td>

    <a href="#" @click.prevent="del(item.id)">删除</a>

  </td>

</tr>

```

2. 在2.x版本中[手动实现筛选的方式](https://cn.vuejs.org/v2/guide/list.html#显示过滤-排序结果)：

+ 筛选框绑定到 VM 实例中的 `searchName` 属性：

```

<hr> 输入筛选名称：

<input type="text" v-model="searchName">

```

+ 在使用 `v-for` 指令循环每一行数据的时候，不再直接 `item in list`，而是 `in` 一个 过滤的methods 方法，同时，把过滤条件`searchName`传递进去：

```

<tbody>

      <tr v-for="item in search(searchName)">

        <td>{{item.id}}</td>

        <td>{{item.name}}</td>

        <td>{{item.ctime}}</td>

        <td>

          <a href="#" @click.prevent="del(item.id)">删除</a>

        </td>

      </tr>

    </tbody>

```

+ `search` 过滤方法中，使用 数组的 `filter` 方法进行过滤：

```

search(name) {

  return this.list.filter(x => {

    return x.name.indexOf(name) != -1;

  });

}

```







## Vue调试工具`vue-devtools`的安装步骤和使用

[Vue.js devtools - 翻墙安装方式 - 推荐](https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd?hl=zh-CN)





## 过滤器

概念：Vue.js 允许你自定义过滤器，**可被用作一些常见的文本格式化**。过滤器可以用在两个地方：**mustache 插值和 v-bind 表达式**。过滤器应该被添加在 JavaScript 表达式的尾部，由“管道”符指示；

### 私有过滤器

1. HTML元素：

```

<td>{{item.ctime | dataFormat('yyyy-mm-dd')}}</td>

```

2. 私有 `filters` 定义方式：

```

filters: { // 私有局部过滤器，只能在 当前 VM 对象所控制的 View 区域进行使用

    dataFormat(input, pattern = "") { // 在参数列表中 通过 pattern="" 来指定形参默认值，防止报错

      var dt = new Date(input);

      // 获取年月日

      var y = dt.getFullYear();

      var m = (dt.getMonth() + 1).toString().padStart(2, '0');

      var d = dt.getDate().toString().padStart(2, '0');



      // 如果 传递进来的字符串类型，转为小写之后，等于 yyyy-mm-dd，那么就返回 年-月-日

      // 否则，就返回  年-月-日 时：分：秒

      if (pattern.toLowerCase() === 'yyyy-mm-dd') {

        return `${y}-${m}-${d}`;

      } else {

        // 获取时分秒

        var hh = dt.getHours().toString().padStart(2, '0');

        var mm = dt.getMinutes().toString().padStart(2, '0');

        var ss = dt.getSeconds().toString().padStart(2, '0');



        return `${y}-${m}-${d} ${hh}:${mm}:${ss}`;

      }

    }

  }

```



> 使用ES6中的字符串新方法 String.prototype.padStart(maxLength, fillString='') 或 String.prototype.padEnd(maxLength, fillString='')来填充字符串；





### 全局过滤器

```

// 定义一个全局过滤器

Vue.filter('dataFormat', function (input, pattern = '') {

  var dt = new Date(input);

  // 获取年月日

  var y = dt.getFullYear();

  var m = (dt.getMonth() + 1).toString().padStart(2, '0');

  var d = dt.getDate().toString().padStart(2, '0');



  // 如果 传递进来的字符串类型，转为小写之后，等于 yyyy-mm-dd，那么就返回 年-月-日

  // 否则，就返回  年-月-日 时：分：秒

  if (pattern.toLowerCase() === 'yyyy-mm-dd') {

    return `${y}-${m}-${d}`;

  } else {

    // 获取时分秒

    var hh = dt.getHours().toString().padStart(2, '0');

    var mm = dt.getMinutes().toString().padStart(2, '0');

    var ss = dt.getSeconds().toString().padStart(2, '0');



    return `${y}-${m}-${d} ${hh}:${mm}:${ss}`;

  }

});

```



> 注意：当有局部和全局两个名称相同的过滤器时候，会以就近原则进行调用，即：局部过滤器优先于全局过滤器被调用！



## 键盘修饰符以及自定义键盘修饰符

### 1.x中自定义键盘修饰符【了解即可】

```

Vue.directive('on').keyCodes.f2 = 113;

```

### [2.x中自定义键盘修饰符](https://cn.vuejs.org/v2/guide/events.html#键值修饰符)

1. 通过`Vue.config.keyCodes.名称 = 按键值`来自定义案件修饰符的别名：

```

Vue.config.keyCodes.f2 = 113;

```

2. 使用自定义的按键修饰符：

```

<input type="text" v-model="name" @keyup.f2="add">

```





## [自定义指令](https://cn.vuejs.org/v2/guide/custom-directive.html)

1. 自定义全局和局部的 自定义指令：

```

    // 自定义全局指令 v-focus，为绑定的元素自动获取焦点：

    Vue.directive('focus', {

      inserted: function (el) { // inserted 表示被绑定元素插入父节点时调用

        el.focus();

      }

    });



    // 自定义局部指令 v-color 和 v-font-weight，为绑定的元素设置指定的字体颜色 和 字体粗细：

      directives: {

        color: { // 为元素设置指定的字体颜色

          bind(el, binding) {

            el.style.color = binding.value;

          }

        },

        'font-weight': function (el, binding2) { // 自定义指令的简写形式，等同于定义了 bind 和 update 两个钩子函数

          el.style.fontWeight = binding2.value;

        }

      }

```

2. 自定义指令的使用方式：

```

<input type="text" v-model="searchName" v-focus v-color="'red'" v-font-weight="900">

```



## Vue 1.x 中 自定义元素指令【已废弃,了解即可】
```
Vue.elementDirective('red-color', {
  bind: function () {
    this.el.style.color = 'red';
  }
});
```
使用方式：
```
<red-color>1232</red-color>
```

## 相关文章
1. [vue.js 1.x 文档](https://v1-cn.vuejs.org/)
2. [vue.js 2.x 文档](https://cn.vuejs.org/)
3. [String.prototype.padStart(maxLength, fillString)](http://www.css88.com/archives/7715)
4. [js 里面的键盘事件对应的键码](http://www.cnblogs.com/wuhua1/p/6686237.html)
5. [Vue.js双向绑定的实现原理](http://www.cnblogs.com/kidney/p/6052935.html)