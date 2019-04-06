# Vue.js - Day2

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

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <script src="./lib/vue-2.4.0.js"></script>
  <link rel="stylesheet" href="./lib/bootstrap-3.3.7.css">
  <!-- 需要用到Jquery吗？？？ -->
</head>

<body>
  <div id="app">




    <div class="panel panel-primary">
      <div class="panel-heading">
        <h3 class="panel-title">添加品牌</h3>
      </div>
      <div class="panel-body form-inline">
        <label>
          Id:
          <input type="text" class="form-control" v-model="id">
        </label>

        <label>
          Name:
          <input type="text" class="form-control" v-model="name">
        </label>

        <!-- 在Vue中，使用事件绑定机制，为元素指定处理函数的时候，如果加了小括号，就可以给函数传参了 -->
        <input type="button" value="添加" class="btn btn-primary" @click="add()">

        <label>
          搜索名称关键字：
          <input type="text" class="form-control" v-model="keywords">
        </label>
      </div>
    </div>



    <table class="table table-bordered table-hover table-striped">
      <thead>
        <tr>
          <th>Id</th>
          <th>Name</th>
          <th>Ctime</th>
          <th>Operation</th>
        </tr>
      </thead>
      <tbody>
        <!-- 之前， v-for 中的数据，都是直接从 data 上的list中直接渲染过来的 -->
        <!-- 现在， 我们自定义了一个 search 方法，同时，把 所有的关键字，通过传参的形式，传递给了 search 方法 -->
        <!-- 在 search 方法内部，通过 执行 for 循环， 把所有符合 搜索关键字的数据，保存到 一个新数组中，返回 -->
        <!--<tr v-for="item in list" :key="item.id">-->
        <tr v-for="item in search(keywords)" :key="item.id">
          <td>{{ item.id }}</td>
          <td v-text="item.name"></td>
          <td>{{ item.ctime }}</td>
          <td>
            <a href="" @click.prevent="del(item.id)">删除</a>
          </td>
        </tr>
      </tbody>
    </table>

  </div>

  <script>
    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {
        id: '',
        name: '',
        keywords: '', // 搜索的关键字
        list: [
          { id: 1, name: '奔驰', ctime: new Date() },
          { id: 2, name: '宝马', ctime: new Date() }
        ]
      },
      methods: {
        add() { // 添加的方法
          // console.log('ok')
          // 分析：
          // 1. 获取到 id 和 name ,直接从 data 上面获取 
          // 2. 组织出一个对象
          // 3. 把这个对象，调用 数组的 相关方法，添加到 当前 data 上的 list 中
          // 4. 注意：在Vue中，已经实现了数据的双向绑定，每当我们修改了 data 中的数据，Vue会默认监听到数据的改动，自动把最新的数据，应用到页面上；

          // 5. 当我们意识到上面的第四步的时候，就证明大家已经入门Vue了，我们更多的是在进行 VM中 Model 数据的操作，同时，在操作Model数据的时候，指定的业务逻辑操作；

          var car = { id: this.id, name: this.name, ctime: new Date() }
          this.list.push(car)
          this.id = this.name = ''
        },
        del(id) { // 根据Id删除数据
          // 分析：
          // 1. 如何根据Id，找到要删除这一项的索引
          // 2. 如果找到索引了，直接调用 数组的 splice 方法

           /*this.list.some((item, i) => {
            if (item.id == id) {
              this.list.splice(i, 1)
              // 在 数组的 some 方法中，如果 return true，就会立即终止这个数组的后续循环
              return true;
            }
          })*/


          var index = this.list.findIndex(item => {
            if (item.id == id) {
              return true;
            }
          })

          // console.log(index)
          this.list.splice(index, 1)  //  删除从 index 开始的1个元素
        },
        search(keywords) { // 根据关键字，进行数据的搜索
          /* var newList = []
          this.list.forEach(item => {
            if (item.name.indexOf(keywords) != -1) {
              newList.push(item)
            }
          })
          return newList */

          // 注意：  forEach   some   filter   findIndex   这些都属于数组的新方法，
          //  都会对数组中的每一项，进行遍历，执行相关的操作；
          return this.list.filter(item => {
            // if(item.name.indexOf(keywords) != -1)

            // 注意 ： ES6中，为字符串提供了一个新方法，叫做  String.prototype.includes('要包含的字符串')
            //  如果包含，则返回 true ，否则返回 false
            //  contain
            if (item.name.includes(keywords)) {
              return item
            }
          })

          //  return itemlist
        }
      }
    });
  </script>
</body>
```

过滤器基本使用：

```html
<body>
  <div id="app">
    <p>{{ msg | msgFormat('疯狂+1', '123') | test }}</p>
  </div>

  <script>
    // 定义一个 Vue 全局的过滤器，名字叫做  msgFormat
    Vue.filter('msgFormat', function (msg, arg, arg2) {
      // 字符串的  replace 方法，第一个参数，除了可写一个 字符串之外，还可以定义一个正则
      return msg.replace(/单纯/g, arg + arg2)
    })

    Vue.filter('test', function (msg) {
      return msg + '========'
    })


    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {
        msg: '曾经，我也是一个单纯的少年，单纯的我，傻傻的问，谁是世界上最单纯的男人'
      },
      methods: {}
    });
  </script>
</body>
```



### 私有过滤器

- 当有多个过滤器时候，会依次调用，并返回最后结果到第一个管道前的变量中

1. HTML元素：

```html
<td>{{item.ctime | dataFormat('yyyy-mm-dd')}}</td>
```

2. 私有 `filters` 定义方式：

```javascript
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

```javascript
// 定义一个全局过滤器
Vue.filter('dataFormat', function (input, pattern = '') {
  var dt = new Date(input);
  // 获取年月日
  var y = dt.getFullYear();
  var m = (dt.getMonth() + 1).toString().padStart(2, '0')
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

```js
Vue.config.keyCodes.f2 = 113;
```

2. 使用自定义的按键修饰符：

```html
<input type="text" v-model="name" @keyup.f2="add">
```

## [自定义指令](https://cn.vuejs.org/v2/guide/custom-directive.html)

1. 自定义全局和局部的 自定义指令：

```js
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

```html
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

### 综合案例

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <script src="./lib/vue-2.4.0.js"></script>
  <link rel="stylesheet" href="./lib/bootstrap-3.3.7.css">
  <!-- 需要用到Jquery吗？？？ -->
</head>

<body>
  <div id="app">
    <!-- {{1+1}} -->
    <div class="panel panel-primary">
      <div class="panel-heading">
        <h3 class="panel-title">添加品牌</h3>
      </div>
      <div class="panel-body form-inline">
        <label>
          Id:
          <input type="text" class="form-control" v-model="id">
        </label>

        <label>
          Name:
          <input type="text" class="form-control" v-model="name" @keyup.f2="add">
        </label>

        <!-- 在Vue中，使用事件绑定机制，为元素指定处理函数的时候，如果加了小括号，就可以给函数传参了 -->
        <input type="button" value="添加" class="btn btn-primary" @click="add()">

        <label>
          搜索名称关键字：
          <!-- 注意： Vue中所有的指令，在调用的时候，都以 v- 开头 -->
          <input type="text" class="form-control" v-model="keywords" id="search" v-focus v-color="'green'">
        </label>
      </div>
    </div>
    <table class="table table-bordered table-hover table-striped">
      <thead>
        <tr>
          <th>Id</th>
          <th>Name</th>
          <th>Ctime</th>
          <th>Operation</th>
        </tr>
      </thead>
      <tbody>
        <!-- 之前， v-for 中的数据，都是直接从 data 上的list中直接渲染过来的 -->
        <!-- 现在， 我们自定义了一个 search 方法，同时，把 所有的关键字，通过传参的形式，传递给了 search 方法 -->
        <!-- 在 search 方法内部，通过 执行 for 循环， 把所有符合 搜索关键字的数据，保存到 一个新数组中，返回 -->
        <tr v-for="item in search(keywords)" :key="item.id">
          <td>{{ item.id }}</td>
          <td v-text="item.name"></td>
          <td>{{ item.ctime | dateFormat() }}</td>
          <td>
            <a href="" @click.prevent="del(item.id)">删除</a>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
  <div id="app2">
    <h3 v-color="'pink'" v-fontweight="900" v-fontsize="50">{{ dt | dateFormat }}</h3>
  </div>
  <script>
    // 全局的过滤器， 进行时间的格式化
    // 所谓的全局过滤器，就是所有的VM实例都共享的
    Vue.filter('dateFormat', function (dateStr, pattern = "") {
      // 根据给定的时间字符串，得到特定的时间
      var dt = new Date(dateStr)

      //   yyyy-mm-dd
      var y = dt.getFullYear()
      var m = dt.getMonth() + 1
      var d = dt.getDate()

      // return y + '-' + m + '-' + d
      if (pattern.toLowerCase() === 'yyyy-mm-dd') {
        return `${y}-${m}-${d}`
      } else {
        var hh = dt.getHours()
        var mm = dt.getMinutes()
        var ss = dt.getSeconds()

        return `${y}-${m}-${d} ${hh}:${mm}:${ss}`
      }
    })
    // 自定义全局按键修饰符
    Vue.config.keyCodes.f2 = 113

    // 使用  Vue.directive() 定义全局的指令  v-focus
    // 其中：参数1 ： 指令的名称，注意，在定义的时候，指令的名称前面，不需要加 v- 前缀, 
    // 但是： 在调用的时候，必须 在指令名称前 加上 v- 前缀来进行调用
    //  参数2： 是一个对象，这个对象身上，有一些指令相关的函数，这些函数可以在特定的阶段，执行相关的操作
    Vue.directive('focus', {
      bind: function (el) { // 每当指令绑定到元素上的时候，会立即执行这个 bind 函数，只执行一次
        // 注意： 在每个 函数中，第一个参数，永远是 el ，表示 被绑定了指令的那个元素，这个 el 参数，是一个原生的JS对象
        // 在元素 刚绑定了指令的时候，还没有 插入到 DOM中去，这时候，调用 focus 方法没有作用
        //  因为，一个元素，只有插入DOM之后，才能获取焦点
        // el.focus()
      },
      inserted: function (el) {  // inserted 表示元素 插入到DOM中的时候，会执行 inserted 函数【触发1次】
        el.focus()
        // 和JS行为有关的操作，最好在 inserted 中去执行，放置 JS行为不生效
      },
      updated: function (el) {  // 当VNode更新的时候，会执行 updated， 可能会触发多次

      }
    })
    // 自定义一个 设置字体颜色的 指令
    Vue.directive('color', {
      // 样式，只要通过指令绑定给了元素，不管这个元素有没有被插入到页面中去，这个元素肯定有了一个内联的样式
      // 将来元素肯定会显示到页面中，这时候，浏览器的渲染引擎必然会解析样式，应用给这个元素
      bind: function (el, binding) {
        // el.style.color = 'red'
        // console.log(binding.name)
        // 和样式相关的操作，一般都可以在 bind 执行

        // console.log(binding.value)
        // console.log(binding.expression)

        el.style.color = binding.value
      }
    })
    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {
        id: '',
        name: '',
        keywords: '', // 搜索的关键字
        list: [
          { id: 1, name: '奔驰', ctime: new Date() },
          { id: 2, name: '宝马', ctime: new Date() }
        ]
      },
      methods: {
        add() { // 添加的方法
          // console.log('ok')
          // 分析：
          // 1. 获取到 id 和 name ,直接从 data 上面获取 
          // 2. 组织出一个对象
          // 3. 把这个对象，调用 数组的 相关方法，添加到 当前 data 上的 list 中
          // 4. 注意：在Vue中，已经实现了数据的双向绑定，每当我们修改了 data 中的数据，Vue会默认监听到数据的改动，自动把最新的数据，应用到页面上；

          // 5. 当我们意识到上面的第四步的时候，就证明大家已经入门Vue了，我们更多的是在进行 VM中 Model 数据的操作，同时，在操作Model数据的时候，指定的业务逻辑操作；

          var car = { id: this.id, name: this.name, ctime: new Date() }
          this.list.push(car)
          this.id = this.name = ''
        },
        del(id) { // 根据Id删除数据
          // 分析：
          // 1. 如何根据Id，找到要删除这一项的索引
          // 2. 如果找到索引了，直接调用 数组的 splice 方法

          /* this.list.some((item, i) => {
            if (item.id == id) {
              this.list.splice(i, 1)
              // 在 数组的 some 方法中，如果 return true，就会立即终止这个数组的后续循环
              return true;
            }
          }) */
          var index = this.list.findIndex(item => {
            if (item.id == id) {
              return true;
            }
          })

          // console.log(index)
          this.list.splice(index, 1)
        },
        search(keywords) { // 根据关键字，进行数据的搜索
          /* var newList = []
          this.list.forEach(item => {
            if (item.name.indexOf(keywords) != -1) {
              newList.push(item)
            }
          })
          return newList */

          // 注意：  forEach   some   filter   findIndex   这些都属于数组的新方法，
          //  都会对数组中的每一项，进行遍历，执行相关的操作；
          return this.list.filter(item => {
            // if(item.name.indexOf(keywords) != -1)

            // 注意 ： ES6中，为字符串提供了一个新方法，叫做  String.prototype.includes('要包含的字符串')
            //  如果包含，则返回 true ，否则返回 false
            //  contain
            if (item.name.includes(keywords)) {
              return item
            }
          })

          // return newList
        }
      }
    });
    // 如何自定义一个私有的过滤器（局部）
    var vm2 = new Vue({
      el: '#app2',
      data: {
        dt: new Date()
      },
      methods: {},
      filters: { // 定义私有过滤器    过滤器有两个 条件  【过滤器名称 和 处理函数】
        // 过滤器调用的时候，采用的是就近原则，如果私有过滤器和全局过滤器名称一致了，这时候 优先调用私有过滤器
        dateFormat: function (dateStr, pattern = '') {
          // 根据给定的时间字符串，得到特定的时间
          var dt = new Date(dateStr)
          //   yyyy-mm-dd
          var y = dt.getFullYear()
          var m = (dt.getMonth() + 1).toString().padStart(2, '0')
          var d = dt.getDate().toString().padStart(2, '0')
          if (pattern.toLowerCase() === 'yyyy-mm-dd') {
            return `${y}-${m}-${d}`
          } else {
            var hh = dt.getHours().toString().padStart(2, '0')
            var mm = dt.getMinutes().toString().padStart(2, '0')
            var ss = dt.getSeconds().toString().padStart(2, '0')
            return `${y}-${m}-${d} ${hh}:${mm}:${ss} ~~~~~~~`
          }
        }
      },
      directives: { // 自定义私有指令
        'fontweight': { // 设置字体粗细的
          bind: function (el, binding) {
            el.style.fontWeight = binding.value
          }
        },
        'fontsize': function (el, binding) { // 注意：这个 function 等同于 把 代码写到了 bind 和 update 中去
          el.style.fontSize = parseInt(binding.value) + 'px'
        }
      }
    })
    // 过滤器的定义语法
    // Vue.filter('过滤器的名称', function(){})

    // 过滤器中的 function ，第一个参数，已经被规定死了，永远都是 过滤器 管道符前面 传递过来的数据
    /* Vue.filter('过滤器的名称', function (data) {
      return data + '123'
    }) */
    // document.getElementById('search').focus()
  </script>
</body>
</html>
<!-- 过滤器调用时候的格式    {{ name | 过滤器的名称 }} -->
```



## [vue实例的生命周期](https://cn.vuejs.org/v2/guide/instance.html#实例生命周期)

+ 什么是生命周期：从Vue实例创建、运行、到销毁期间，总是伴随着各种各样的事件，这些事件，统称为生命周期！
+ [生命周期钩子](https://cn.vuejs.org/v2/api/#选项-生命周期钩子)：就是生命周期事件的别名而已；
+ 生命周期钩子 = 生命周期函数 = 生命周期事件
+ 主要的生命周期函数分类：
 - 创建期间的生命周期函数：
    + beforeCreate：实例刚在内存中被创建出来，此时，还没有初始化好 data 和 methods 属性
    	+ created：实例已经在内存中创建OK，此时 data 和 methods 已经创建OK，此时还没有开始 编译模板
    	+ beforeMount：此时已经完成了模板的编译，但是还没有挂载到页面中
    	+ mounted：此时，已经将编译好的模板，挂载到了页面指定的容器中显示
 - 运行期间的生命周期函数：
    + beforeUpdate：状态更新之前执行此函数， 此时 data 中的状态值是最新的，但是界面上显示的 数据还是旧的，因为此时还没有开始重新渲染DOM节点
    	+ updated：实例更新完毕之后调用此函数，此时 data 中的状态值 和 界面上显示的数据，都已经完成了更新，界面已经被重新渲染好了！
 - 销毁期间的生命周期函数：
    + beforeDestroy：实例销毁之前调用。在这一步，实例仍然完全可用。
      + destroyed：Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。


### 综合代码

```html
<body>
  <div id="app">
    <input type="button" value="修改msg" @click="msg='No'">
    <h3 id="h3">{{ msg }}</h3>
  </div>

  <script>
    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {
        msg: 'ok'
      },
      methods: {
        show() {
          console.log('执行了show方法')
        }
      },
      beforeCreate() { // 这是我们遇到的第一个生命周期函数，表示实例完全被创建出来之前，会执行它
        // console.log(this.msg)
        // this.show()
        // 注意： 在 beforeCreate 生命周期函数执行的时候，data 和 methods 中的 数据都还没有没初始化
      },
      created() { // 这是遇到的第二个生命周期函数
        // console.log(this.msg)
        // this.show()
        //  在 created 中，data 和 methods 都已经被初始化好了！
        // 如果要调用 methods 中的方法，或者操作 data 中的数据，最早，只能在 created 中操作
      },
      beforeMount() { // 这是遇到的第3个生命周期函数，表示 模板已经在内存中编辑完成了，但是尚未把 模板渲染到 页面中
        // console.log(document.getElementById('h3').innerText)
        // 在 beforeMount 执行的时候，页面中的元素，还没有被真正替换过来，只是之前写的一些模板字符串
      },
      mounted() { // 这是遇到的第4个生命周期函数，表示，内存中的模板，已经真实的挂载到了页面中，用户已经可以看到渲染好的页面了
        // console.log(document.getElementById('h3').innerText)
        // 注意： mounted 是 实例创建期间的最后一个生命周期函数，当执行完 mounted 就表示，实例已经被完全创建好了，此时，如果没有其它操作的话，这个实例，就静静的 躺在我们的内存中，一动不动
      },


      // 接下来的是运行中的两个事件
      beforeUpdate() { // 这时候，表示 我们的界面还没有被更新【数据被更新了吗？  数据肯定被更新了】
        /* console.log('界面上元素的内容：' + document.getElementById('h3').innerText)
        console.log('data 中的 msg 数据是：' + this.msg) */
        // 得出结论： 当执行 beforeUpdate 的时候，页面中的显示的数据，还是旧的，此时 data 数据是最新的，页面尚未和 最新的数据保持同步
      },
      updated() {
        console.log('界面上元素的内容：' + document.getElementById('h3').innerText)
        console.log('data 中的 msg 数据是：' + this.msg)
        // updated 事件执行的时候，页面和 data 数据已经保持同步了，都是最新的
      }
    });
  </script>
```



## [vue-resource 实现 get, post, jsonp请求](https://github.com/pagekit/vue-resource)
除了 vue-resource 之外，还可以使用 `axios` 的第三方包实现实现数据的请求

```html
<head>
  <script src="./lib/vue-2.4.0.js"></script>
  <!-- 注意：vue-resource 依赖于 Vue，所以先后顺序要注意  -->
  <!-- this.$http.jsonp -->
  <script src="./lib/vue-resource-1.3.4.js"></script>  <!--必须导入的包-->
</head>
<body>
  <div id="app">
    <input type="button" value="get请求" @click="getInfo">
    <input type="button" value="post请求" @click="postInfo">
    <input type="button" value="jsonp请求" @click="jsonpInfo">
  </div>

  <script>
    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {},
      methods: {
        getInfo() { // 发起get请求
          //  当发起get请求之后， 通过 .then 来设置成功的回调函数
          this.$http.get('http://vue.studyit.io/api/getlunbo').then(function (result) {
            // 通过 result.body 拿到服务器返回的成功的数据
            // console.log(result.body)
          })
        },
        postInfo() { // 发起 post 请求   application/x-wwww-form-urlencoded
          //  手动发起的 Post 请求，默认没有表单格式，所以，有的服务器处理不了
          //  通过 post 方法的第三个参数， { emulateJSON: true } 设置 提交的内容类型 为 普通表单数据格式
          this.$http.post('http://vue.studyit.io/api/post', {}, { emulateJSON: true }).then(result => {
            console.log(result.body)
          })
        },
        jsonpInfo() { // 发起JSONP 请求
          this.$http.jsonp('http://vue.studyit.io/api/jsonp').then(result => {
            console.log(result.body)
          })
        }
      }
    });
  </script>
</body>
```

node服务端的app.js

```js
// 导入 http 内置模块
const http = require('http')
// 这个核心模块，能够帮我们解析 URL地址，从而拿到  pathname  query 
const urlModule = require('url')

// 创建一个 http 服务器
const server = http.createServer()
// 监听 http 服务器的 request 请求
server.on('request', function (req, res) {

  // const url = req.url
  const { pathname: url, query } = urlModule.parse(req.url, true)

  if (url === '/getscript') {
    // 拼接一个合法的JS脚本，这里拼接的是一个方法的调用
    // var scriptStr = 'show()'

    var data = {
      name: 'xjj',
      age: 18,
      gender: '女孩子'
    }

    var scriptStr = `${query.callback}(${JSON.stringify(data)})`
    // res.end 发送给 客户端， 客户端去把 这个 字符串，当作JS代码去解析执行
    res.end(scriptStr)
  } else {
    res.end('404')
  }
})

// 指定端口号并启动服务器监听
server.listen(3000, function () {
  console.log('server listen at http://127.0.0.1:3000')
})
```

1. 之前的学习中，如何发起数据请求？
2. 常见的数据请求类型？  get  post jsonp
3. 测试的URL请求资源地址：
 + get请求地址： http://vue.studyit.io/api/getlunbo
 + post请求地址：http://vue.studyit.io/api/post
 + jsonp请求地址：http://vue.studyit.io/api/jsonp
4. JSONP的实现原理
 + 由于浏览器的安全性限制，不允许AJAX访问 协议不同、域名不同、端口号不同的 数据接口，浏览器认为这种访问不安全；
 + 可以通过动态创建script标签的形式，把script标签的src属性，指向数据接口的地址，因为script标签不存在跨域限制，这种数据获取方式，称作JSONP（注意：根据JSONP的实现原理，知晓，JSONP只支持Get请求）；
 + 具体实现过程：
		- 先在客户端定义一个回调方法，预定义对数据的操作；
		- 再把这个回调方法的名称，通过URL传参的形式，提交到服务器的数据接口；
		- 服务器数据接口组织好要发送给客户端的数据，再拿着客户端传递过来的回调方法名称，拼接出一个调用这个方法的字符串，发送给客户端去解析执行；
		- 客户端拿到服务器返回的字符串之后，当作Script脚本去解析执行，这样就能够拿到JSONP的数据了；
 + 带大家通过 Node.js ，来手动实现一个JSONP的请求例子；
 ```
    const http = require('http');
    // 导入解析 URL 地址的核心模块
    const urlModule = require('url');

    const server = http.createServer();
    // 监听 服务器的 request 请求事件，处理每个请求
    server.on('request', (req, res) => {
      const url = req.url;

      // 解析客户端请求的URL地址
      var info = urlModule.parse(url, true);

      // 如果请求的 URL 地址是 /getjsonp ，则表示要获取JSONP类型的数据
      if (info.pathname === '/getjsonp') {
        // 获取客户端指定的回调函数的名称
        var cbName = info.query.callback;
        // 手动拼接要返回给客户端的数据对象
        var data = {
          name: 'zs',
          age: 22,
          gender: '男',
          hobby: ['吃饭', '睡觉', '运动']
        }
        // 拼接出一个方法的调用，在调用这个方法的时候，把要发送给客户端的数据，序列化为字符串，作为参数传递给这个调用的方法：
        var result = `${cbName}(${JSON.stringify(data)})`;
        // 将拼接好的方法的调用，返回给客户端去解析执行
        res.end(result);
      } else {
        res.end('404');
      }
    });

    server.listen(3000, () => {
      console.log('server running at http://127.0.0.1:3000');
    });
 ```
5. vue-resource 的配置步骤：
 + 直接在页面中，通过`script`标签，引入 `vue-resource` 的脚本文件；
 + 注意：引用的先后顺序是：先引用 `Vue` 的脚本文件，再引用 `vue-resource` 的脚本文件；
6. 发送get请求：
```
getInfo() { // get 方式获取数据
  this.$http.get('http://127.0.0.1:8899/api/getlunbo').then(res => {
    console.log(res.body);
  })
}
```
7. 发送post请求：
```
postInfo() {
  var url = 'http://127.0.0.1:8899/api/post';
  // post 方法接收三个参数：
  // 参数1： 要请求的URL地址
  // 参数2： 要发送的数据对象
  // 参数3： 指定post提交的编码类型为 application/x-www-form-urlencoded
  this.$http.post(url, { name: 'zs' }, { emulateJSON: true }).then(res => {
    console.log(res.body);
  });
}
```
8. 发送JSONP请求获取数据：
```
jsonpInfo() { // JSONP形式从服务器获取数据
  var url = 'http://127.0.0.1:8899/api/jsonp';
  this.$http.jsonp(url).then(res => {
    console.log(res.body);
  });
}
```

### Veu请求根路径的配置

```js
// 如果我们通过全局配置了，请求的数据接口 根域名，则 ，在每次单独发起 http 请求的时候，请求的 url 路径，应该以相对路径开头，前面不能带 /  ，否则 不会启用根路径做拼接；
    Vue.http.options.root = 'http://vue.studyit.io/';
 // 全局启用 emulateJSON 选项
    Vue.http.options.emulateJSON = true;
```

完整HTML代码

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <script src="./lib/vue-2.4.0.js"></script>
  <script src="./lib/vue-resource-1.3.4.js"></script>
  <link rel="stylesheet" href="./lib/bootstrap-3.3.7.css">
</head>

<body>
  <div id="app">



    <div class="panel panel-primary">
      <div class="panel-heading">
        <h3 class="panel-title">添加品牌</h3>
      </div>
      <div class="panel-body form-inline">

        <label>
          Name:
          <input type="text" v-model="name" class="form-control">
        </label>

        <input type="button" value="添加" @click="add" class="btn btn-primary">
      </div>
    </div>



    <table class="table table-bordered table-hover table-striped">
      <thead>
        <tr>
          <th>Id</th>
          <th>Name</th>
          <th>Ctime</th>
          <th>Operation</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in list" :key="item.id">
          <td>{{item.id}}</td>
          <td>{{item.name}}</td>
          <td>{{item.ctime}}</td>
          <td>
            <a href="" @click.prevent="del(item.id)">删除</a>
          </td>
        </tr>
      </tbody>
    </table>


  </div>

  <script>
    // 如果我们通过全局配置了，请求的数据接口 根域名，则 ，在每次单独发起 http 请求的时候，请求的 url 路径，应该以相对路径开头，前面不能带 /  ，否则 不会启用根路径做拼接；
    Vue.http.options.root = 'http://vue.studyit.io/';
    // 全局启用 emulateJSON 选项
    Vue.http.options.emulateJSON = true;

    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {
        name: '',
        list: [ // 存放所有品牌列表的数组
        ]
      },
      created() { // 当 vm 实例 的 data 和 methods 初始化完毕后，vm实例会自动执行created 这个生命周期函数
        this.getAllList()
      },
      methods: {
        getAllList() { // 获取所有的品牌列表 
          // 分析：
          // 1. 由于已经导入了 Vue-resource这个包，所以 ，可以直接通过  this.$http 来发起数据请求
          // 2. 根据接口API文档，知道，获取列表的时候，应该发起一个 get 请求
          // 3. this.$http.get('url').then(function(result){})
          // 4. 当通过 then 指定回调函数之后，在回调函数中，可以拿到数据服务器返回的 result
          // 5. 先判断 result.status 是否等于0，如果等于0，就成功了，可以 把 result.message 赋值给 this.list ; 如果不等于0，可以弹框提醒，获取数据失败！

          this.$http.get('api/getprodlist').then(result => {
            // 注意： 通过 $http 获取到的数据，都在 result.body 中放着
            var result = result.body
            if (result.status === 0) {
              // 成功了
              this.list = result.message
            } else {
              // 失败了
              alert('获取数据失败！')
            }
          })
        },
        add() {  // 添加品牌列表到后台服务器
          // 分析：
          // 1. 听过查看 数据API接口，发现，要发送一个 Post 请求，  this.$http.post
          // 2. this.$http.post() 中接收三个参数：
          //   2.1 第一个参数： 要请求的URL地址
          //   2.2 第二个参数： 要提交给服务器的数据 ，要以对象形式提交给服务器 { name: this.name }
          //   3.3 第三个参数： 是一个配置对象，要以哪种表单数据类型提交过去， { emulateJSON: true }, 以普通表单格式，将数据提交给服务器 application/x-www-form-urlencoded
          // 3. 在 post 方法中，使用 .then 来设置成功的回调函数，如果想要拿到成功的结果，需要 result.body

          /* this.$http.post('api/addproduct', { name: this.name }, { emulateJSON: true }).then(result => {
            if (result.body.status === 0) {
              // 成功了！
              // 添加完成后，只需要手动，再调用一下 getAllList 就能刷新品牌列表了
              this.getAllList()
              // 清空 name 
              this.name = ''
            } else {
              // 失败了
              alert('添加失败！')
            }
          }) */

          this.$http.post('api/addproduct', { name: this.name }).then(result => {
            if (result.body.status === 0) {
              // 成功了！
              // 添加完成后，只需要手动，再调用一下 getAllList 就能刷新品牌列表了
              this.getAllList()
              // 清空 name 
              this.name = ''
            } else {
              // 失败了
              alert('添加失败！')
            }
          })
        },
        del(id) { // 删除品牌
          this.$http.get('api/delproduct/' + id).then(result => {
            if (result.body.status === 0) {
              // 删除成功
              this.getAllList()
            } else {
              alert('删除失败！')
            }
          })
        }
      }
    });
  </script>
</body>

</html>
```



## 配置本地数据库和数据接口API

1. 先解压安装 `PHPStudy`;
2. 解压安装 `Navicat` 这个数据库可视化工具，并激活；
3. 打开 `Navicat` 工具，新建空白数据库，名为 `dtcmsdb4`;
4. 双击新建的数据库，连接上这个空白数据库，在新建的数据库上`右键` -> `运行SQL文件`，选择并执行 `dtcmsdb4.sql` 这个数据库脚本文件；如果执行不报错，则数据库导入完成；
5. 进入文件夹 `vuecms3_nodejsapi` 内部，执行 `npm i` 安装所有的依赖项；
6. 先确保本机安装了 `nodemon`, 没有安装，则运行 `npm i nodemon -g` 进行全局安装，安装完毕后，进入到 `vuecms3_nodejsapi`目录 -> `src`目录 -> 双击运行 `start.bat`
7. 如果API启动失败，请检查 PHPStudy 是否正常开启，同时，检查 `app.js` 中第 `14行` 中数据库连接配置字符串是否正确；PHPStudy 中默认的 用户名是root，默认的密码也是root

## 品牌管理改造
### 展示品牌列表

### 添加品牌数据

### 删除品牌数据

## [Vue中的动画](https://cn.vuejs.org/v2/guide/transitions.html)https://cn.vuejs.org/v2/guide/transitions.html
为什么要有动画：动画能够提高用户的体验，帮助用户更好的理解页面中的功能；

### 使用过渡类名
1. HTML结构：
```html
<div id="app">
    <input type="button" value="动起来" @click="myAnimate">
    <!-- 使用 transition 将需要过渡的元素包裹起来 -->
    <transition name="fade">  <!--需要自定义样式的时候，只需要将transition的name修改成自己的-->
      <div v-show="isshow">动画哦</div>
    </transition>
  </div>
```
2. VM 实例：
```js
// 创建 Vue 实例，得到 ViewModel
var vm = new Vue({
  el: '#app',
  data: {
    isshow: false
  },
  methods: {
    myAnimate() {
      this.isshow = !this.isshow;
    }
  }
});
```
3. 定义两组类样式：
```js
/* 定义进入和离开时候的过渡状态 */
    .fade-enter-active,
    .fade-leave-active {
      transition: all 0.2s ease;
      position: absolute;
    }

    /* 定义进入过渡的开始状态 和 离开过渡的结束状态 */
    .fade-enter,
    .fade-leave-to {
      opacity: 0;
      transform: translateX(100px);
    }
```

案例：

```html
<script src="./lib/vue-2.4.0.js"></script>
  <!-- 2. 自定义两组样式，来控制 transition 内部的元素实现动画 -->
  <style>
    /* v-enter 【这是一个时间点】 是进入之前，元素的起始状态，此时还没有开始进入 */
    /* v-leave-to 【这是一个时间点】 是动画离开之后，离开的终止状态，此时，元素 动画已经结束了 */
    .v-enter,
    .v-leave-to {
      opacity: 0;
      transform: translateX(150px);
    }

    /* v-enter-active 【入场动画的时间段】 */
    /* v-leave-active 【离场动画的时间段】 */
    .v-enter-active,
    .v-leave-active{
      transition: all 0.8s ease;
    }
  </style>
</head>

<body>
  <div id="app">
    <input type="button" value="toggle" @click="flag=!flag">
    <!-- 需求： 点击按钮，让 h3 显示，再点击，让 h3 隐藏 -->
    <!-- 1. 使用 transition 元素，把 需要被动画控制的元素，包裹起来 -->
    <!-- transition 元素，是 Vue 官方提供的 -->
    <transition>
      <h3 v-if="flag">这是一个H3</h3>
    </transition>
  </div>

  <script>
    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {
        flag: false
      },
      methods: {}
    });
  </script>
</body>
```



### [使用第三方 CSS 动画库](https://cn.vuejs.org/v2/guide/transitions.html#自定义过渡类名)

1. 导入动画类库：
```
<link rel="stylesheet" type="text/css" href="./lib/animate.css">
```
2. 定义 transition 及属性：
```html
<div id="app">
    <input type="button" value="toggle" @click="flag=!flag">
    <!-- 需求： 点击按钮，让 h3 显示，再点击，让 h3 隐藏 -->
    <!-- <transition enter-active-class="animated bounceIn" leave-active-class="animated bounceOut">
      <h3 v-if="flag">这是一个H3</h3>
    </transition> -->

    <!-- 使用 :duration="毫秒值" 来统一设置 入场 和 离场 时候的动画时长 -->
    <!-- <transition enter-active-class="bounceIn" leave-active-class="bounceOut" :duration="200">
      <h3 v-if="flag" class="animated">这是一个H3</h3>
    </transition> -->

    <!-- 使用  :duration="{ enter: 200, leave: 400 }"  来分别设置 入场的时长 和 离场的时长  -->
    <transition 
    enter-active-class="bounceIn" 
    leave-active-class="bounceOut" 
    :duration="{ enter: 200, leave: 400 }">
      <h3 v-if="flag" class="animated">这是一个H3</h3>
    </transition> 
  </div>
<script>
    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {
        flag: false
      },
      methods: {}
    });
  </script>
```

### 使用动画钩子函数
1. 定义 transition 组件以及三个钩子函数：
```html
  <script src="./lib/vue-2.4.0.js"></script>
  <style>
    .ball {
      width: 15px;
      height: 15px;
      border-radius: 50%;
      background-color: red;
    }
  </style>
</head>

<body>
  <div id="app">
    <input type="button" value="快到碗里来" @click="flag=!flag">
    <!-- 1. 使用 transition 元素把 小球包裹起来 -->
    <transition
      @before-enter="beforeEnter"
      @enter="enter"
      @after-enter="afterEnter">
      <div class="ball" v-show="flag"></div>
    </transition>
  </div>

  <script>

    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {
        flag: false
      },
      methods: {
        // 注意： 动画钩子函数的第一个参数：el，表示 要执行动画的那个DOM元素，是个原生的 JS DOM对象
        // 大家可以认为 ， el 是通过 document.getElementById('') 方式获取到的原生JS DOM对象
        beforeEnter(el){
          // beforeEnter 表示动画入场之前，此时，动画尚未开始，可以 在 beforeEnter 中，设置元素开始动画之前的起始样式
          // 设置小球开始动画之前的，起始位置
          el.style.transform = "translate(0, 0)"
        },
        enter(el, done){
          // 这句话，没有实际的作用，但是，如果不写，出不来动画效果；
          // 可以认为 el.offsetWidth 会强制动画刷新
          el.offsetWidth
          // enter 表示动画 开始之后的样式，这里，可以设置小球完成动画之后的，结束状态
          el.style.transform = "translate(150px, 450px)"
          el.style.transition = 'all 1s ease'

          // 这里的 done， 起始就是 afterEnter 这个函数，也就是说：done 是 afterEnter 函数的引用
          done()
        },
        afterEnter(el){
          // 动画完成之后，会调用 afterEnter
          // console.log('ok')
          this.flag = !this.flag
        }
      }
    });
  </script>
</body>
```
2. 定义三个 methods 钩子方法：
```js
methods: {
        beforeEnter(el) { // 动画进入之前的回调
          el.style.transform = 'translateX(500px)';
        },
        enter(el, done) { // 动画进入完成时候的回调
          el.offsetWidth;
          el.style.transform = 'translateX(0px)';
          done();
        },
        afterEnter(el) { // 动画进入完成之后的回调
          this.isshow = !this.isshow;
        }
      }
```
3. 定义动画过渡时长和样式：
```js
.show{
      transition: all 0.4s ease;  //  这儿是定义的过度的样式
    }
```


### [v-for 的列表过渡](https://cn.vuejs.org/v2/guide/transitions.html#列表的进入和离开过渡)
1. 定义过渡样式：
```
<style>
    .list-enter,
    .list-leave-to {
      opacity: 0;
      transform: translateY(10px);
    }

    .list-enter-active,
    .list-leave-active {
      transition: all 0.3s ease;
    }
</style>
```
2. 定义DOM结构，其中，需要使用 transition-group 组件把v-for循环的列表包裹起来：
```
  <div id="app">
    <input type="text" v-model="txt" @keyup.enter="add">

    <transition-group tag="ul" name="list">
      <li v-for="(item, i) in list" :key="i">{{item}}</li>
    </transition-group>
  </div>
```
3. 定义 VM中的结构：
```
    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {
        txt: '',
        list: [1, 2, 3, 4]
      },
      methods: {
        add() {
          this.list.push(this.txt);
          this.txt = '';
        }
      }
    });
```

案例：

```html
<script src="./lib/vue-2.4.0.js"></script>
  <style>
    li {
      border: 1px dashed #999;
      margin: 5px;
      line-height: 35px;
      padding-left: 5px;
      font-size: 12px;
      width: 100%;
    }

    li:hover {
      background-color: hotpink;
      transition: all 0.8s ease;
    }



    .v-enter,
    .v-leave-to {
      opacity: 0;
      transform: translateY(80px);
    }

    .v-enter-active,
    .v-leave-active {
      transition: all 0.6s ease;
    }

    /* 下面的 .v-move 和 .v-leave-active 配合使用，能够实现列表后续的元素，渐渐地漂上来的效果 */
    .v-move {
      transition: all 0.6s ease;
    }
    .v-leave-active{
      position: absolute;
    }
  </style>
</head>

<body>
  <div id="app">

    <div>
      <label>
        Id:
        <input type="text" v-model="id">
      </label>

      <label>
        Name:
        <input type="text" v-model="name">
      </label>

      <input type="button" value="添加" @click="add">
    </div>

    <!-- <ul> -->
      <!-- 在实现列表过渡的时候，如果需要过渡的元素，是通过 v-for 循环渲染出来的，不能使用 transition 包裹，需要使用 transitionGroup -->
      <!-- 如果要为 v-for 循环创建的元素设置动画，必须为每一个 元素 设置 :key 属性 -->
      <!-- 给 ransition-group 添加 appear 属性，实现页面刚展示出来时候，入场时候的效果 -->
      <!-- 通过 为 transition-group 元素，设置 tag 属性，指定 transition-group 渲染为指定的元素，如果不指定 tag 属性，默认，渲染为 span 标签 -->
      <transition-group appear tag="ul">
        <li v-for="(item, i) in list" :key="item.id" @click="del(i)">
          {{item.id}} --- {{item.name}}
        </li>
      </transition-group>
    <!-- </ul> -->

  </div>

  <script>
    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {
        id: '',
        name: '',
        list: [
          { id: 1, name: '赵高' },
          { id: 2, name: '秦桧' },
          { id: 3, name: '严嵩' },
          { id: 4, name: '魏忠贤' }
        ]
      },
      methods: {
        add() {
          this.list.push({ id: this.id, name: this.name })
          this.id = this.name = ''
        },
        del(i) {
          this.list.splice(i, 1)
        }
      }
    });
  </script>
</body>
```



### 列表的排序过渡

`<transition-group>` 组件还有一个特殊之处。不仅可以进入和离开动画，**还可以改变定位**。要使用这个新功能只需了解新增的 `v-move` 特性，**它会在元素的改变定位的过程中应用**。
+ `v-move` 和 `v-leave-active` 结合使用，能够让列表的过渡更加平缓柔和：
```js
.v-move{
  transition: all 0.8s ease;
}
.v-leave-active{
  position: absolute;
}
```



## 相关文章
1. [vue.js 1.x 文档](https://v1-cn.vuejs.org/)
2. [vue.js 2.x 文档](https://cn.vuejs.org/)
3. [String.prototype.padStart(maxLength, fillString)](http://www.css88.com/archives/7715)
4. [js 里面的键盘事件对应的键码](http://www.cnblogs.com/wuhua1/p/6686237.html)
5. [pagekit/vue-resource](https://github.com/pagekit/vue-resource)
6. [navicat如何导入sql文件和导出sql文件](https://jingyan.baidu.com/article/a65957f4976aad24e67f9b9b.html)
7. [贝塞尔在线生成器](http://cubic-bezier.com/#.4,-0.3,1,.33)