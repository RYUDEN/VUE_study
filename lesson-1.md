# vue基础-lesson 1

> 学习笔记,复习笔记
>  第一天


## 数据绑定

### html数据绑定

数据绑定使用用 '{{}}','v-html','v-text' 绑定 



```
<!-- 可以绑定数据的表达式  如:{{name.replace('vue','javascript')}} -->
<p>{{name}}</p> 
	<!-- 第一种方式 双括号 -->
<p v-text="name"></p>	 
	<!-- 第二种方式 v-text -->
<p v-html="name"></p>
	<!-- 第三种方式 v-html	 (数据中包含html便签,可以渲染出来) -->
<script>
	new Vue({
		data:{

			name:"vue基础"
		}
	})
</script>
```		 

### html属性绑定

一些常用的html属性可以使用 v-bind (简写 ':') 来进行绑定

如  class  src 等等 属性

使用v-bind:class=""  v-bind:src="" 等等绑定 元素的属性.简写为  " :class= " ," :src= "

![](https://cn.vuejs.org/images/logo.png)
```
<img :src="link">
<script>
	new Vue({
		data:{
			link:'https://cn.vuejs.org/images/logo.png'
		}
	})
</script>

```

### dom对象的事件绑定

使用 v-on  例如:v-on:click="change"  为元素绑定 click 事件. change 为触发事件的处理方法

```

<a @click="change"></a> 

<script >
// 点击事件触发 change方法/函数
	new Vue({
		//函数/方法写在 Vue对象的methods对象里面.
		methods:{
			change:function(){
				alert('你好!')
			}
		}
	})
</script>

```

##  条件渲染

###  v-if  和 v-show 

v-if="object"  若 data对象里面的数据 object存在 或者为 true 的时候,渲染该标签;
v-show="object"  object 存在 或者为true 的时候渲染  但是若不为true的时候仅仅改变display属性为none;

v-if 可以和 v-else-if ,v-else等 连用  v-show不可以.

```
<p v-if="obj">show</p>
<p v-show="obj">show</p>

<script>
	new Vue({
		data:{
			obj:true
		}
	})
</script>

```


## 列表渲染  v-for


v-for 用于渲染 多条同级数据 ,相同类型数据 .也可以和v-if连用,进行筛选.
v-for可以传入 index ,index为item在当前数组当中的位置.

```

<body>
    <div id="app">
        <div v-for="item in gift">
        <!-- <div v-for="(item,index) in gift"></div> 传入了index值 可以在每一项当中进行访问 -->
        	<!-- 根据数组进行渲染	item指数组中每一项  -->
            <button v-if="item.status == 1"> 锁定 </button>
            <!-- <p>{{index}}</p> 渲染为index序号 -->
            <!-- 连用的时候筛选 status的不同取值 进行不同的渲染 -->
            <button v-else-if="item.status == 2"> 解锁 </button>

            <button v-else="item.status == 3"> 提交 </button>
        </div>
        <p>{{xixi}}</p>
        <input v-model="xixi">
    </div>
</body>
<script>
    new Vue({
        el:"#app",
        data:{
            xixi:'',
            gift:[
                {
                    status:1
                },
                {
                    status:2
                },
                {
                    status:3
                },
                   
            ]
        },
        methods:{
        }
    })
</script>

```

## v-model 输入框数据绑定


v-model 用于实时监听输入框输入文字.实现数据和输入框的双向绑定

```

<p>{{message}}</p>
<!-- 将message数据绑定到 input和p标签上面  当更改输入框内容  p标签内容实时更改,无需其他操作,实现数据双向绑定 -->
<input v-model="message">
<script>
	new Vue({
		data:{
			message:''
		}
	})
</script>

```



