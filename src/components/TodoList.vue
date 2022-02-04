<template>
  <ul class="todo-main">
    <TodoListItem 
      v-for="todo in todos" 
      :key="todo.id" 
      :todo="todo" 
      :checkTodo="checkTodo" 
      :removeTodo="removeTodo"
    />
  </ul>
</template>
<script>
import TodoListItem from './TodoListItem'

export default {
  name: 'TodoList',
  components: {
    TodoListItem
  },
  props: ['todos', 'checkTodo', 'removeTodo']
}
</script>
<style scoped>
	/*main*/
	.todo-main {
		margin-left: 0px;
		border: 1px solid #ddd;
		border-radius: 2px;
		padding: 0px;
	}

	.todo-empty {
		height: 40px;
		line-height: 40px;
		border: 1px solid #ddd;
		border-radius: 2px;
		padding-left: 5px;
		margin-top: 10px;
	}
</style>