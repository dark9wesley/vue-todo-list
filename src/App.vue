<template>
  <div id="root">
    <div class="todo-container">
      <div class="todo-wrap">
        <TodoHeader @addTodo="addTodo" />
        <TodoList :todos="todos"/>
        <TodoFooter :todos="todos" @checkAllTodo="checkAllTodo" @removeCheckedTodo="removeCheckedTodo" />
      </div>
    </div>
  </div>
</template>

<script>
import TodoHeader from './components/TodoHeader'
import TodoFooter from './components/TodoFooter'
import TodoList from './components/TodoList'

export default {
  name: 'App',
  components: {
    TodoHeader,
    TodoFooter,
    TodoList,
  },
  data(){
    return {
      todos: JSON.parse(localStorage.getItem('todos')) || []
    }
  },
  methods: {
    // 添加一个todo
    addTodo(todo){
      this.todos.unshift(todo)
    },
    // 勾选or取消勾选一个todo
    checkTodo(id){
      this.todos.forEach(todo => {
        if(todo.id === id) todo.done = !todo.done
      })
    },
    // 勾选or取消勾选全部todo
    checkAllTodo(bool){
      this.todos.forEach(todo => {
        todo.done = bool
      })
    },
    // 删除一个Todo
    removeTodo(id){
      const newTodos = this.todos.filter(todo => todo.id !== id)
      this.todos = newTodos
    },
    // 删除全部已完成Todo
    removeCheckedTodo(){
      const newTodos =  this.todos.filter(todo => !todo.done)
      this.todos = newTodos
    },
    // 将一个Todo更改为编辑模式
    handleEdit(id){
      this.todos.forEach(todo => {
        if(todo.id !== id) return
        if(Object.prototype.hasOwnProperty.call(todo, 'isEdit')){
          todo.isEdit = true
        }else{
          this.$set(todo, 'isEdit', true)
        }
      })
    },
    // 确认编辑内容
    confirmEdit(id, value){
      this.todos.forEach(todo => {
        if(todo.id !== id) return
        todo.value = value
        todo.isEdit = false
      })
    }
  },
  watch: {
    todos: {
      deep: true,
      handler(newTodos){
        localStorage.setItem('todos', JSON.stringify(newTodos))
      }
    }
  },
  mounted(){
    this.$bus.$on('checkTodo', this.checkTodo)
    this.$bus.$on('removeTodo', this.removeTodo)
    this.$bus.$on('handleEdit', this.handleEdit)
    this.$bus.$on('confirmEdit', this.confirmEdit)
  },
  beforeDestroy(){
    this.$bus.$off(['checkTodo', 'removeTodo', 'handleEdit', 'confirmEdit'])
  }
}
</script>

<style>
  /*base*/
  body {
    background: #fff;
  }

  .btn {
    display: inline-block;
    padding: 4px 12px;
    margin-bottom: 0;
    font-size: 14px;
    line-height: 20px;
    text-align: center;
    vertical-align: middle;
    cursor: pointer;
    box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
    border-radius: 4px;
  }

  .btn-danger {
    color: #fff;
    background-color: #da4f49;
    border: 1px solid #bd362f;
  }

  .btn-edit {
    color: #fff;
    background-color: skyblue;
    border: 1px solid skyblue;
    margin-right: 10px;
  }

  .btn-danger:hover {
    color: #fff;
    background-color: #bd362f;
  }

  .btn:focus {
    outline: none;
  }

  .todo-container {
    width: 600px;
    margin: 0 auto;
  }
  .todo-container .todo-wrap {
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 5px;
  }

</style>
