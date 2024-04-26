<script>
export default {
  data () {
    return {
      todos:[
        {
          id:123123,
          title:'test content1',
          completed: false
        },
        {
          id:103710,
          title:'test content2',
          completed:true
        },
      ],
      visibility:'all',
      isFooterVisible : true
      //设置footer的显示与否
    }
  },
  methods:{
    toggleAll(e){
      this.todos.forEach(todo=>todo.completed=e.target.checked)
    },
    onHashChange(){
      const hash = window.location.hash.replace(/#\/?/,'')
      if (['all','active','completed'].includes(hash)){
        this.visibility = hash
      }else{
        window.location.hash = ''
        this.visibility = 'all'
      }
    },
    addTodo(e){
      const title = e.target.value.trim()
      if (!title){
        return 
      }
      this.todos.push(
        {id:Date.now(),
        title,
        completed:false      
        }
      )
      e.target.value=''
      this.isFooterVisible = true
    },
    Delete(id){
      // console.log(id)
      this.todos=this.todos.filter(todo=>todo.id!==id)
    },
    clearCompleted(){
      this.todos=this.todos.filter(todo=>todo.completed!==true)
      this.isFooterVisible = this.todos.length === 0 ? false : true;
      
    }
  },
  computed:{
    filterTodo (){
      switch(this.visibility){
        case 'all' : return this.todos
        case 'active': return this.todos.filter(todo=>!todo.completed)
        case 'completed': return this.todos.filter(todo=>todo.completed)
      }

    },
    remaining(){
      return this.todos.filter(todo=>!todo.completed).length
    },
    // isFooterVisible(){
    //   return this.remaining>0
    // }
  },
  //生命周期钩子函数
  mounted (){
    window.addEventListener('hashchange',this.onHashChange)
    //好吧这里仅仅是将路由定在之前的状态而不是恢复为默认值all
    this.onHashChange()
  }
  
}
      

</script>

<template>
  <section class="todoapp">
			<header class="header">
				<h1>todos</h1>
				<input @keyup.enter = "addTodo" class="new-todo" placeholder="What needs to be done?" autofocus>
			</header>
			<!-- This section should be hidden by default and shown when there are todos -->
			<section class="main">
				<input id="toggle-all" class="toggle-all" type="checkbox" @click="toggleAll">
				<label for="toggle-all">Mark all as complete</label>
				<ul class="todo-list">
					<!-- These are here just to show the structure of the list items -->
					<!-- List items should get the class `editing` when editing and `completed` when marked as completed -->
					<li
          v-for = "todo in filterTodo"
          :key="todo.id"
          :class="{completed:todo.completed}">
						<div class="view">
							<input class="toggle" type="checkbox" v-model="todo.completed">
              <!-- v-model双向绑定，输入框的值影响页面渲染 ：class这是单向绑定 -->
              <!-- 这个第一个注释错误了，其实这是个checkbox的双向绑定，不是上面的文本框 -->
							<label v-text="todo.title"></label>
							<button class="destroy" @click="Delete(todo.id)"></button>
						</div>
						<input class="edit" value="Create a TodoMVC template">
					</li>
				</ul>
			</section>
			<!-- This footer should be hidden by default and shown when there are todos -->
			<footer class="footer" v-if="isFooterVisible">
				<!-- This should be `0 items left` by default -->
				<span class="todo-count">
          <strong>
            {{remaining}}
          </strong>
          item{{ remaining >1 ? 's' : '' }} left</span>
				<!-- Remove this if you don't implement routing -->
				<ul class="filters">
					<li>
						<a :class="{selected:visibility==='all'}" href="#/all">All</a>
					</li>
					<li>
						<a :class="{selected:visibility==='active'}"href="#/active">Active</a>
					</li>
					<li>
						<a :class="{selected:visibility==='completed'}"href="#/completed">Completed</a>
					</li>
          <!-- select是用于高亮的 -->
				</ul>
				<!-- Hidden if no completed items are left ↓ -->
				<button class="clear-completed" @click="clearCompleted">Clear completed</button>
			</footer>
		</section>
</template>

<style >
@import "https://unpkg.com/todomvc-app-css@2.4.1/index.css";
</style>
