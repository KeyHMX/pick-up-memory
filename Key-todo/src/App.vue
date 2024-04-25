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
      visibility:'all'
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
    }
  },
  computed:{
    filterTodo (){
      switch(this.visibility){
        case 'all' : return this.todos
        case 'active': return this.todos.filter(todo=>!todo.completed)
        case 'completed': return this.todos.filter(todo=>todo.completed)
      }

    }
  },
  mounted (){
    window.addEventListener('hashchange',this.onHashChange)
  }
  
}
      

</script>

<template>
  <section class="todoapp">
			<header class="header">
				<h1>todos</h1>
				<input class="new-todo" placeholder="What needs to be done?" autofocus>
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
							<button class="destroy"></button>
						</div>
						<input class="edit" value="Create a TodoMVC template">
					</li>
				</ul>
			</section>
			<!-- This footer should be hidden by default and shown when there are todos -->
			<footer class="footer">
				<!-- This should be `0 items left` by default -->
				<span class="todo-count"><strong>0</strong> item left</span>
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
				</ul>
				<!-- Hidden if no completed items are left ↓ -->
				<button class="clear-completed">Clear completed</button>
			</footer>
		</section>
</template>

<style >
@import "https://unpkg.com/todomvc-app-css@2.4.1/index.css";
</style>
