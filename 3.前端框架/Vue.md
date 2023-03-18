# Vue

**什么是Vue？**

一个前端框架，专注于数据处理，双向数据绑定。视图<--->数据。



## 第一个Vue示例

先创建视图，再创建Vue对象关联视图

el：元素，定位视图。

data：显示在视图里的数据。

```vue
<body>
		<!--创建视图-->
		<div id="app">
			{{rawHtml}}
		</div>
		<!--再创建vue的对象关联视图-->
		<script type="text/javascript">
			var vue = new Vue({
				el:"#app",
				data:{
					rawHtml:"今天是<b>圣诞节</b>~",
					myclass:"bg",
					num:100,
					ok:1
                    msg:"vue"
				}
			});
		</script>
	</body>
```



## 模板语法插值

**不解析HTML：** `v-text=""`

**解析HTML：** `v-html=""`

**绑定样式：**`<p v-bind:class="myclass">test</p>`

**算术运算：**`{{num+100}}`

**三元运算：**`{{ok==1?"yes":"no"}}` 注意：如果不加双引号是取变量

**函数调用：**`{{msg.split('').reverse().join('')}}`//输出:euv



## 模板语法指令

**v-if**：条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。

v-if是惰性，如果在初始渲染时条件为假，则什么都不做。



**v-show**：条件渲染，如果渲染时为假，隐藏该标签。



**v-for数组**：循环输出数组

```vue
<h2>v-for数组</h2>
	<ul>
		<li v-for="(item,i) in menuList">
		{{i+1}}-----{{item.name}}-----{{item.price}}
		</li>
	</ul>
```



**v-for对象**：循环输出对象

```vue
<ul>
	<li v-for="(v,k) in stu">
		{{k}}---{{v}}
	</li>
</ul>
```



## 事件处理-单向数据

```vue
<style type="text/css">
	.active{
		color: red;
			}		
</style>

//标签上绑定样式的引用
<p v-bind:class="{active:flag}">我是显示颜色的</p>
<button @click="change">改变颜色</button>
<button @click="flag=!flag">改变颜色</button>

<script type="text/javascript">
	var vue = new Vue({
		el:"#app",
		data:{
		mycolor:"active",
		flag:false
				},
		methods:{
			change(){
				//改变段落的颜色
                //取反赋值，模拟开关
				this.flag=!this.flag;
				}
			}
	});
</script>
```



## 事件处理-双向数据绑定

**v-model:**

```vue
<body>
		<div id="app">
			<!-- 只有表单元素(有value的属性)才有双向绑定 -->
			直接显示数据：-----&nbsp;{{msg}}
			
			
			<!-- 解析双向绑定 
			  v-bind可缩写为:
			  v-on  可缩写为@
			-->
			<h2>双向绑定解析</h2>
			表单显示数据：<input v-bind:value="msg"/>
			双向显示数据：<input v-model="msg"/>
			
		</div>
		<script type="text/javascript">
			var vue = new Vue({
				el:"#app",
				data:{
					msg:"原始数据"
				}
			});
		</script>
		
</body>
```



## 组件开发

```vue
<script type="text/javascript">
	//1.创建子组件
	var App = {
	template:"<div>入口组件</div>"
			}
	//创建父组件
	var vue = new Vue({
		el:"#app",
		//3.使用子组件
		template:"<App/>",
		//2.挂载子组件
		components:{
			App
			}
		});
</script>
```

