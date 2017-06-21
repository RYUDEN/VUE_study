# lesson-2 用vue.js 制作一个 todolist 

>学习笔记  第二天  

## 需求分析

1. 实现输入框输入之后,点击添加或者敲击回车键 将输入内容添加进入 div容器。添加完毕之后清空输入框。
2. 点击 选项以后，高亮显示选项。再次点击取消高亮。点击选项后面的删除可以删除 特定的选项。
3. 实时将容器中的各个选项储存在浏览器localStorage当中，关闭浏览器再次打开直接展示内容。


## 技术分析 

1. 使用 v-model 绑定数据。
2. 使用 v-for 渲染列表。
3. 使用 v-on 为选项 绑定点击事件，更改选项状态。
4. 使用 Vue的观察属性，观察列表变化 ，实时将数据储存到localStorage。（新）
5. 使用 Vue的created方法，在打开浏览器的时候去读取保存的内容。（新）

## 观察属性  watch 


在Vue实例当中使用观察属性，可以观察Vue当中的变化。/n
在 watch 属性当中，键为 需要观察的对像，值为 对应的回调方法，包含选项的对象。
当做出更改的时候，Vue实例会去调用$watch 遍历每一个watch对象中的属性。

```
// vuejs官网对 watch属性的一个解释。。
 var vm = new Vue({
  data: {
    a: 1,
    b: 2,
    c: 3
  },
  watch: {
    a: function (val, oldVal) {
      console.log('new: %s, old: %s', val, oldVal)
    },
    // 方法名
    b: 'someMethod',
    // 深度 watcher
    c: {
      handler: function (val, oldVal) { /* ... */ },
      deep: true
    }
  }
})
vm.a = 2 // -> new: 2, old: 1

```
## created方法  

created 方法在 该Vue实例创建时调用的的方法。
本次示例当中，在created方法当中执行 调用储存。

```

created：function(){
        this.tips = JSON.parse(window.localStorage.getItem('todo'))
        //创建VUE示例的时候，data里面tips数据为空，读取保存的 ‘todo’去填充 tips  
}
```


## 代码分析



```
    <div id="test" >
        <h4>{{list.test}}</h4>

        <input type="text" v-model="list.test" @keyup.enter="add" placeholder="请输入" >
        <!-- 为input标签绑定一个test数据，实时捕捉输入的数据。绑定了一个 ‘keyup.enter’事件，当回车键敲击的时候，执行add方法。 -->

        <button @click="add">add+</button>
        <!-- 为按钮绑定一个 'click'点击事件,点击触发 add方法 -->
            <p v-for="(tip,index) in tips" :class="{styl:tip.tipStatus}" @click=“change(index)”>{{tip.test}} <span @click="remove(index)">remove</span></p>
            <!-- 使用 v-for 指令,循环遍历 tips当中的数据 ,循环当中传入第二个属性'index',用于操作 删除方法. 为remove按钮绑定了一个 remove（）方法 ，传入 index 值，删除相关的选项。为每一个选项绑定点击事件，触发change方法。从而改变item状态。-->
    </div>

  <script>
    var Wx= new Vue({
        el:'#test',
        data:{
            tips:[], //用于存储 每一个item。
            list:{
	       	    test:'this is a  to do list',//输入框绑定的值。
	            tipStatus:false//每个tip的状态信息	
            }
        },
        methods:{
            add:function () {
            	//add添加方法。如果有输入内容，才可以触发添加。添加之后还原输入框为空。
                if(Wx.list.test){
                    Wx.tips.push(list);
                    Wx.list.test = ''
                }
            },
            remove:function (index) {
            	//remove 移除选项方法。传入index值，去删除相应的值。
                Wx.tips.splice(index,1)
            },
            change:function(index){
            	//为选项传入 index值，可以更改所点击的对象的状态。
            	Wx.tips[index].tipStatus = !Wx.tips[index].tipStatus
            }
        },
        created:function () {
        	//从localStorage当中读取保存的数据。 	

            this.tips = JSON.parse(window.localStorage.getItem('todo'))
        },
        watch:{
            tips: {

            	//观察属性 观察tips数组的变化 。使用watch属性当中的深度观察方法。每一次的变化都将会去执行这次方法。

                handler:function (){
                    var todo = Wx.tips;
                    window.localStorage.setItem('todo',JSON.stringify(todo))
                },
                //deep选项控制是不是深度观察。这里为true
                deep:true
            }
        }
    })
</script>