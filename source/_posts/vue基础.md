---
title: vue基础
date: 2021-06-21 07:44:46
tags: [前端]
---

### methods_弹窗

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8"></meta>
    <title>index</title>
<script src="https://unpkg.com/vue"> </script>

</head>
<body>


    <div id ="app">
        <!---生成一个按钮-->
        <button v-on:click="sayHi"> click me </button>

    </div>
    <script> 
        var vm = new Vue({
            el: "#app",
            //model: 数据模型，泛指后端进行的各种业务逻辑和数据操控，mvvm
            data: {
                message:"this'a a blog"
            },
            //事件
            methods: { //方法必须定义在Vue的Method对象中,为了达到监听的效果，必须传递event参数
                sayHi: function(event){
                    alert(this.message);
                }

            }
        });
    </script>
</body>
</html>
```

### 双向数据绑定

>view　通过 <input  `v-model`= "item">将用户的输入绑定到名外item的数据上
>
>​			通过{{item}},绑定显示后台名为item的数据
>
>​	

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8"></meta>
    <title>index</title>
<script src="https://unpkg.com/vue"> </script>

</head>
<body>


    <div id="watch-example">
        <p>
            Ask a yes/no question:
            <input v-model="question">
        </p>
        <p> 
            {{ answer }}
        </p>
    </div>


<script src="https://cdn.jsdelivr.net/npm/axios@0.12.0/dist/axios.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/lodash@4.13.1/lodash.min.js"></script>
<script>
    var watchExampleVM = new Vue({
        el: '#watch-example',
        data: {
        question: '',
        answer: 'I cannot give you an answer until you ask a question!'
    },
    watch: {
    // 如果 `question` 发生改变，这个函数就会运行
        question: function (newQuestion, oldQuestion) {
            this.answer = 'Waiting for you to stop typing...'
            this.debouncedGetAnswer()
        }   
    },
    created: function () {
    // `_.debounce` 是一个通过 Lodash 限制操作频率的函数。
    // 在这个例子中，我们希望限制访问 yesno.wtf/api 的频率
    // AJAX 请求直到用户输入完毕才会发出。想要了解更多关于
    // `_.debounce` 函数 (及其近亲 `_.throttle`) 的知识，
    // 请参考：https://lodash.com/docs#debounce
        this.debouncedGetAnswer = _.debounce(this.getAnswer, 500)
    },
    methods: {
        getAnswer: function () {
            if (this.question.indexOf('?') === -1) {
                this.answer = 'Questions usually contain a question mark-"?" . ;)'
            return
        }
        this.answer = 'Thinking...'
        var vm = this
        axios.get('https://yesno.wtf/api')
            .then(function (response) {
            vm.answer = _.capitalize(response.data.answer)
            })
            .catch(function (error) {
            vm.answer = 'Error! Could not reach the API. ' + error
        })
    }
  }
})
</script>

</body>
</html>
```

### vue组件

用户自定义的结构

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8"></meta>
    <title>index</title>
<script src="https://unpkg.com/vue"> </script>

</head>
<body>
    <div id="app">
        <!--ｖ-bind后面不能加空格 -->
        <!--v-bind:a=b,将实参b与形参a绑定，类似于函数调用，这里item是v-for遍历的结果-->
        <qinjiang v-for="item in items" v-bind:qin="item"> </qinjiang>
    </div>
    
    
    <script> 
       //定义一个vue组件
        Vue.component('qinjiang',{
                //props:定一个形参，　template：利用{{}}，取形参中的值来显示一些内容
                props: ['qin'],  
                template: '<li>{{qin}}</li>'
        });

        var vm = new Vue({
            el: "#app",
            //model: 数据模型，泛指后端进行的各种业务逻辑和数据操控，mvvm
            data: {
                items: ["java","linux","前端"]
            }
        });
    </script>
</body>
</html>
```

### Axios异步通信

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8"></meta>
    <title>index</title>
<script src="https://unpkg.com/vue"> </script>
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>

</head>
<body>
    <div id="vue">
        点我
        <div>
            {{info.name}}
        </div>
        <div>
            {{info.address.street}}
        </div>
    </div>

    <script type="text/javascript"> 
        var vm = new Vue({
            el: '#vue',
            data(){
                return{
                    //请求的返回参数格式必须和json串一样,return的只是一个函数，data()方法会自己捕获
                    info:{
                        name : null
                        address:{
                               street: null,
                                city: null,
                                country: null
                        }
                    }
                }
            },
            //
            mounted(){//钩子函数
                axios.get('data.json').then(response=>(this.info=response.data));

            }
        })
    </script>
</body>
</html>
```

对应的json文件`data.json`

```json
{
  "name" : "狂神说java",
  "url" : "htp://baidu.com",
  "page" : 1,
  "isNonProfit" : true,
  "address" : {
    "street": "含光门",
    "city" : "陕西西安",
    "country": "中国"
  },
  "links": [
    {
      "name" : "B站",
      "url": "https://www.bilibili.com/"
    },
    {
      "name": "4399",
      "url": "https://www.4399.com/"
    },
    {
      "name": "百度",
      "url": "https://www.baidu.com/"
    }
  ]
}


```

### 计算属性

计算属性的重点突出在属性两个字上，首先它是一个属性，其次它有计算的能力，这里`计算`就是一个函数；它能够将计算结果缓存起来．

​	内存中执行，虚拟Dao

计算属性的主要特征就是为了将不经常变化的计算结果进行缓存，以节约我们的系统开销．

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8"></meta>
    <title>index</title>
<script src="https://unpkg.com/vue"> </script>
<!-- <script src="https://unpkg.com/axios/dist/axios.min.js"></script> -->

</head>
<body>
    <div id="vue">
        <p> {{currentTime1()}}</p>
        <p> {{currentTime2}}</p>
    </div>

    <script type="text/javascript"> 
        var vm = new Vue({
            el: '#vue',
            data:{
                message: "hello!!"
            },
            methods:{ //函数,调用时需要带括号
                currentTime1: function(){
                    return Date.now();
                }
            },
            //method中的方法名和computed中的属性名不建议一致，methods优先级更好
            computed:　{
                currentTime2: function(){
                    return Date.now(); //计算属性，只有第一次被调用时，进行计算，之后直接在内存中查找结果
                }
            }
        })
    </script>
</body>
</html>
```

### VUE插槽(slot)//内容分发

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8"></meta>
    <title>index</title>
<script src="https://unpkg.com/vue"> </script>
<!-- <script src="https://unpkg.com/axios/dist/axios.min.js"></script> -->

</head>
<body>
    <div id="vue">
        <todo>
            <todo-title slot="todo-title" :title="title"></todo-title>
            <todo-items slot="todo-items" v-for="item in todoItems" :item="item"></todo-items>
        </todo>
    </div>

    <script type="text/javascript"> 
       Vue.component("todo",{
            template: '<div>\
                            <slot name="todo-title"></slot>\
                            <ul>\
                                <slot name="todo-items"> </slot>\
                            </ul>\
                        </div>'
       });
       //通过指定solt的name来绑定组件
       Vue.component("todo-title",{
           props:['title'],
           template:'<div>{{title}}</div>'
       });

       Vue.component("todo-items",{
            props:['item'],
            template: '<li>{{item}}</li>'
       });
        
       var vm = new Vue({
            el:"#vue",
            data:{
                title: "what do you like?",
                todoItems: ["happiness","movie","runnig","reading"]
            }
       });
    </script>
</body>
</html>
```



### 自定义事件

 通过以上代码不难发现，数据项在Vue的实例中，但删除操作要在组件中完成，那么组件如何才能删除Vue实例中的数据呢?此时就涉及到参数传递与事件分发了，Vue为我们提供了自定义事件的功能很好的帮助我们解决了这个问题;

 使用`this.$emit (‘自定义事件名’,参数)`

![vue1](../../vue/vue基础/vue1.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8"></meta>
    <title>index</title>
<script src="https://unpkg.com/vue"> </script>
<!-- <script src="https://unpkg.com/axios/dist/axios.min.js"></script> -->

</head>
<body>
    <div id="vue">
        <todo>
            <todo-title slot="todo-title" :title="title"></todo-title>
            <todo-items slot="todo-items" v-for="(item,index) in todoItems" 
                :item="item" v-bind:index="index" v-on:remove="removeItems(index)" ></todo-items>
        </todo>
    </div>
    <!--：是v-bind的简写，通过v-on将vue中的函数和组件中的自定义方法进行绑定，key是绑定remove中用到的参数-->
    <script type="text/javascript"> 
       Vue.component("todo",{
            template: '<div>\
                            <slot name="todo-title"></slot>\
                            <ul>\
                                <slot name="todo-items"> </slot>\
                            </ul>\
                        </div>'
       });
       //通过指定solt的name来绑定组件
       Vue.component("todo-title",{
           props:['title'],
           template:'<div>{{title}}</div>'
       });

       Vue.component("todo-items",{
            props:['item','index'],
            template: '<li>{{item}}     <button @click="remove"> delete </button></li>' ,
            methods: {
                remove: function (index){
                    this.$emit('remove',index) //这里的remove的名称就是自定义的函数名
                    // alert("111");
                }
            }

       });

       var vm = new Vue({
            el:"#vue",
            data:{
                title: "what do you like?",
                todoItems: ["happiness","movie","runnig","reading"]
            },
            methods:{
                removeItems: function(index){
                    this.todoItems.splice(index,1);//利用javascript中的语法
                }
            }
           
       });
    </script>
</body>
</html>
```

