
## 一、vue属性

### 1.el

​		标记挂载的对象

### 2.methods

​		在里面放一些方法

### 3.computed

​		计算属性，基本和方法没什么区别，但是计算属性如果值没有发生变化的话只会计算一次，可以提高性能

### 4.components

​		相当于一个独立的Vue类，里面几乎可以有Vue有的所有属性 

​		用户声明组件

### 5.watch

​		监听值的变化，注意这里的所有方法名必须和你要监视的变量名一致才行，例如要监视name值的变化方法如下

```javascript
data:{
    name:"Handsome Wu"
},
watch: {
    name(newValue,oldValue){
        //do Something
    }
}
```



## 二、vue 语法糖

v-on: -> @
v-show
v-if
v-for
v-bind: -> :

## 三、传参问题

​		传入本身事件使用 ==$event== 或是 直接不传，vue 会自动给第一个没传的参数赋值为 event 事件。

## 四、修饰符

### v-on 的修饰符

1. 阻止事件冒泡直接使用 stop 修饰符，用法：==@click.stop="方法名"==
2. 阻止默认事件用 provent 修饰符，用法：==@click.prevent="方法名"==
3. 监听键盘事件 enter 修饰符，用法：==@click.enter="方法名"==

### v-modal 的修饰符

1. 实现非实时加载 lazy 修饰符，用法==v-modal.lazy="data"==对于绑定的数据不会实时更新，只有按下回车或失去焦点后会更新
2. 对于 input 的输入类型返回值一定是 string 类型，如果需要对 input 的值进行`string to int`可以使用 number 修饰符，用法：==v-modal.number="age"==
3. 如果想要输入框里空格被忽略可以使用 trim 修饰符，用法：==v-modal.trim="uname"==

## 五、响应式布局

​		对于数组，以下方法可以实现响应式

```java
pop()
push()
shift()     //删除第一个
sort()      //
reverse()   //
unshift()   //在最前面加元素，一次可以加多个
splice()    //删除元素，也可以用来替换
```

 ==但是直接对值进行修改并不能实现响应式==

 ## 六、组件

### 1.创建构造器对象

​		可以使用`符号，这个的好处就是可以换行，适合打html标签

​		注意每次创建只能有一个根节点，意思就是最外层只能有一个

### 2.注册组件

​		注册组件只需要使用Vue.component()，第一个参数为在html里面用的时候的标签名，第二个标签是之前创建的对象

### 3.使用组件

​		在vue接管的下面可以直接使用，html里面用注册过的组件名即可，如：`<handsomewu></handsomewu>`

### 4.示例代码

```javascript
//创建构造器对象，注意只能有一个根节点
//也可以在组件里面创建组件，这样就可以在组件里面用组件了
const cpnC = Vue.extend({
  template: `
  	<div>
     <div>
       <input />
     </div>
     <div>
       <span>内容</span>
	   <cpnSon></cpnSon>
     </div>
   </div>
   `,
   components:{
     cpnSon:cpnSon
   }
})
//注册组件，以下方法为全局组件，在所有vue实例中都可以使用
Vue.component('handsomewu', cpnC)
//注册局部组件，直接在vue实例里面注册，components属性中写即可，只能在被注册的vue实例中使用
```

**语法糖**

```javascript
//全局组件
//不需要额外写extend了，在component内部会自己实现extend
Vue.component('handsomewu', {
  template: `
  	<div>
     <div>
       <input />
     </div>
     <div>
       <span>内容</span>
	   <cpnSon></cpnSon>
     </div>
   </div>
   `,
   components:{
     cpnSon:cpnSon
   }
})

//局部组件，直接在组件里面写即可
const app = new Vue({
      el: '#app',
      data: {
        message: 'hello world',
      },
      methods: {},
      components: {
        cpn1: {
          template: `
          <div>
            <div>
              <input />
            </div>
            <div>
              <input />
            </div>
          </div>
          `,
          components: {
            cpnson: cpnson
          }
        }
      }
    })

```

**分离写法**

```javascript
//script分离写法，注意类型是text-x-template 
<script id="cpn" type="text-x-template">
    <div>
      <h1>mehemehe</h1>
    </div>
</script>
//直接写template，使用时只需要把id放在组件中即可
<template id="cpn">
  <div>
    <h1>mehe</h1>
  </div>
</template>

//调用方法
<script>
    const app = new Vue({
      el: '#app',
      components: {
        cpn: {
          template: '#cpn'				//直接调用id
        }
      }
    })
  </script>
```

### 5.组件数据

```html
//组件大致相当于一个封闭的vue对象，也有methods属性和data属性，可以在这两个里面存放数据以及数据操作
<div id='app'>
    <cpn></cpn>
  </div>
  <template id="cpn">
    <div>
      <h1>当前计数:{{count}}</h1>
      <button @click="add()">+</button>
      <button @click="minus()">-</button>
    </div>
  </template>
  <script>
//这里采用的是全局写法
    Vue.component('cpn', {
      template: '#cpn',
      data() {
        return {
          count: 0
        }
      },
      methods: {
        add() {
          this.count++
        },
        minus() {
          this.count--
        }
      }
    })
    const app = new Vue({
      el: '#app',
      data: {
        message: 'hello world',
      },
      methods: {},
      components: {

      }
    })
  </script>
```

​		说明：关于组件的data属性，由于组件具有复用性，所以组件的data必须是一个函数式的返回值，这样可以在一个组件改变数据时防止其他组件的值跟着一起改变。

### 6.组件通信

#### （1）父传子

​		组件通信主要通过prop属性来实现，可以在里面传入一个对象

​		简单点的话也可以用数组赋值给props，但是一般不用

**代码示例**

```html
<div id='app'>
    <!--驼峰命名法的话在传递的时候不能直接使用，要用-分隔-->
  <cpn :cname="name" :cmessage="message" :c-age="age"></cpn>
</div>
<template id="cpn">
  <div>
    <h1>{{cmessage}}</h1>
  </div>
</template>
<script>
  const cpn = {
    template: '#cpn',
    props: {
      cAge:Number,
      cname:{
        type:Array,
        default(){
          return [1,2,3,4,5,6]    //Array和对象类型的default值需要使用函数的方式设置
        }
      }
    }
  }
  const app = new Vue({
    el: '#app',
    data: {
      message: 'hello world',
      name: ['a', 'b'],
      age:10
    },
    methods: {},
    components: {
      cpn: cpn
    }
  })
</script>
```

#### （2）子传父

```html
<div id='app'>
  <!--绑定自定义事件传递的父函数，test没写参数会自动把event事件传入，也就是item-->
  <cpn @item-click="test"></cpn>				
</div>
<template id="cpn">
  <div>
    <button @click="btnClick()">+</button>
  </div>
</template>
<script>
  const cpn = {
    template: '#cpn',
    methods: {
      btnClick(item) {
        //传递自定义函数,item表示点击事件
        this.$emit('item-click',item)		//自定义事件，名字随便定义
      }
    }
  }
  const app = new Vue({
    el: '#app',
    methods: {
      test() {
        console.log(event.target)
      }
    },
    components: {
      cpn: cpn
    }
  })
</script>
```

​		避免直接在子组件里面修改父组件里面的变量值，如果需要的话不要绑定到prop里面，建议在组件的data里面用一个变量获取到再修改组件里面的data

### 7.组件直接获取对象

#### （1）父获取子

​		第一种为使用==this.$children==，可以直接获取所有子元素，然后再通过下标来获取到指定元素；返回数组

​		第二种为==this.$refs==获取，可以获取所有给了ref属性的标签（推荐使用），只能获取到注册过的元素；返回对象

​		真实开发中一般采用$refs方法，因为开发极有可能会有后期插入的情况，如果使用下标的话一旦修改代码就不可用了

**代码示例**

```html
<div id='app'>
  <cpn></cpn>
  <!-- 注册子元素 -->
  <cpn ref="child"></cpn>         
  <button @click="getChildren()">获取</button>
</div>
<template id="cpn">
  <div>
    <h1>{{cmessage}}</h1>
  </div>
</template>
<script>
  const cpn = {
    template: '#cpn',
    props: {
      cmessage: String
    },
    methods: {
      btnClick(item) {
        this.$emit('item-click', item)
      }
    }
  }
  const app = new Vue({
    el: '#app',
    data: {
      message: 'hello world',
    },
    methods: {
      getChildren() {
        console.log(this.$children)     //可以获取到所有子元素
        console.log(this.$refs.child)   //只能获取到注册了的子元素，注册过的直接用.name即可获得
      }
    },
    components: {
      cpn: cpn
    }
  })
</script>
```

#### （2）子获取父

​		在组件方法里面直接使用==this.$parent==，不建议使用

​		访问根组件用==this.$root==

### 8.插槽slot

相当于占位符放在<template>里面，需要替换的元素写在组件里面会自动替换所有不具名的插槽

**具名插槽**

给每个插槽加一个name属性，然后在替换时给标签加上slot属性，属性值为name，这样可以精准替换指定的插槽

```html
<div id='app'>
  <cpn>
    <button>传入会覆盖不具名插槽的默认值</button>
    <button slot="sname"><i>替换了具名插槽</i></button>
  </cpn>
  <hr>
  <cpn></cpn>
</div>
<template id="cpn">
  <div>
    <h1>slot使用</h1>
    <slot name="sname"><b>这是具名插槽的默认值</b></slot>
    <slot><i>这是默认值</i></slot>
  </div>
</template>
<script>
  const cpn = {
    template: '#cpn',
  }
  const app = new Vue({
    el: '#app',
    data: {
      message: 'hello world',
    },
    components: {
      cpn: cpn
    }
  })
</script>
```

**父组件替换子组件但使用子组件内容作为数据**

​		在slot上绑定data，data即要展示的数据，然后在替换时通过==slot-scope属性==可以获取到data对象，在替换处修改样式即可

在slot-scope里面定义的就是获取到的绑定的数据

```html
<div id='app'>
  <!-- 原始方式展示数据 -->
  <cpn></cpn>
  <!-- 替换方式展示数据 -->
  <cpn>
    <!-- vue2.5.x要求写template，版本高的可以不需要 -->
    <template slot-scope="slot">
      <span v-for="item in slot.data">{{item}} * </span>
    </template>
  </cpn>
</div>
<template id="cpn">
  <div>
    <slot :data="planguage">
      <ul>
        <li v-for="item in planguage">{{item}}</li>
      </ul>
    </slot>
  </div>
</template>
<script>
  const cpn = {
    template: '#cpn',
    data() {
      return {
        planguage: ["java", "c++", "python"],
      }
    }
  }
  const app = new Vue({
    el: '#app',
    data: {
      message: 'hello world',
    },
    components: {
      cpn: cpn
    }
  })
</script>
```

**效果如图**

![image-20210712113622862](C:\Users\Handsome Wu\AppData\Roaming\Typora\typora-user-images\image-20210712113622862.png)

## 七、js文件模块化

### 1.why 模块化

​		防止因为未进行模块化而导致的命名冲突问题，var是全局属性，如果引入的js文件中有相同的命名，那就会产生命名冲突，程序出错。

### 2.原生方法

​		直接在引入的时候把他的类型定义为模块，这样每个js文件在导入时都不会把自己定义的变量作为全局变量，都只是局部变量

```html
<script src="./main.js" type="modual"></script>
```

​		但是以上的导入方法会导致代码没有复用性，即完全不能访问其他js文件的变量或者方法，因此可以在js文件里面加一个导出，把所有需要导出的都放在里面，这样在其他的js文件中只要导入就能使用导出的变量或方法

**导出方法&导入方法：**

```javascript
import * from "./main.js"
//获取指定的导出，需要加中括号
import {name,age} from "./main.js"
//获取默认导出，名字可以自定义
import my_print from "./main.js"
//-----------------------分隔符---------------------------//
//export可以导出任意类型的东西
export {name,age}；
//默认导出，不需要写函数名或变量名，但只能导出一个
export default function (arg){
    console.log(arg)
}
```

### 3.webpack

​		webpack 用于打包文件，这样在本地可以使用commonjs等浏览器不支持的语法

​		一般webpack都是打包一个js文件，其他用到的包只要在这个js文件里面引用就好了

#### （1）webpack打包

```
webpack js路径 打包后路径
```

#### （2）webpack.config.js配置

```javascript
//webpack.config.js配置
const path = require('path')

module.exports={
    //入口就是需要需要打包的js的路径
  entry:'./src/main.js',
  output:{
      //打包后路径
    path:path.resolve(__dirname,'dist'),
    filename:'bundle.js'
  }
}
```

​		如果需要快捷方式可以在package.json的script里面添加自己的快捷语句，用这个方法可以优先在本地查找方法，如果直接打包用的就是全局配置

#### （3）打包其他类型文件

​		首先需要安装一些`loader`，具体已经写在==use字段==里面，一定要==注意版本==！！！！！！！！

​		把需要用到的css文件直接在main.js里面require进去即可

​		然后在webpack.config.js文件里面配置

​		一定注意，use数组的数据顺序有关处理顺序，而且顺序为==从右向左（简便写法），从下到上（详细写法）==

```javascript
const path = require('path')
module.exports={
  entry:'./src/main.js',
  output:{
    path:path.resolve(__dirname,'dist'),
    filename:'bundle.js'
  },
  module: {
  rules: [
      //处理css文件
      {
    	test: /\.css$/,
    	use: ['style-loader', 'css-loader']
  	  },
      //处理less文件
      {
    	test: /\.less$/,
    	use: ['style-loader', 'css-loader','less-loader']
  	  },
      //处理图片文件，如果css里面有用到背景图片也要用
      {
    	test: /\.(png|jpg|jpeg|gif)$/,
    	use: [
            {
                loader:'url-loader',
                //加载图片大于限制值时，直接使用file-loader模块加载，会改图片名
                //加载图片小于限制值时，图片转成base64字节码传输
                limit:13000,
                //file-loader情况下，默认用哈希散列作为文件名，以下为设置文件名的方式（不直接使用hash散列）
                //生成img文件夹下的文件，文件名为原文件名+八位hash值+扩展名，[]不能去掉，否则认为是定值
                name:'img/[name].[hash:8].[ext]'
            }
        ]
  	  }
  ]
  }
}
```

**为什么要给img加hash值？**

​		项目里图片不一定都是存在同一个文件夹下的，可能在img文件下还有不同的文件夹，因此图片名称可能有一致的情况，加了hash值可以防止错误发生

#### （4）ES6转ES5

普通的打包并不会把ES6转化为ES5，对浏览器适配不太好

==安装babel==

```
npm install --save-dev babel-loader@7 babel-core babel-preset-es2015
```

==使用bable==

```javascript
{
    test: /\.js$/,
    //对于node_module中的js文件不需要转化
    exclude: /(node_modules|bower_components)/,
    use: {
      loader: 'babel-loader',
      options: {
        presets: ['es2015']
      }
    }
}
```

#### （5）打包vue代码

如果是runtime-only的话，需要在webpack.config.js中配置

```javascript
const path = require('path')

module.exports = {
  entry: './src/main.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  },
  module: {
    rules: [
        //省略
    ]
  },
  resolve: {
    alias: {
      'vue$': 'vue/dist/vue.esm.js'
    }
  }
}
```

## 八、脚手架