<template>
  <ul class="todo-main">
		<transition-group name='todo' appear>
			<TodoListItem 
				v-for="todo in todos" 
				:key="todo.id" 
				:todo="todo" 
			/>
		</transition-group>
  </ul>
</template>
<script>
import TodoListItem from './TodoListItem'

export default {
  name: 'TodoList',
  components: {
    TodoListItem
  },
  props: ['todos']
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

	.todo-enter-active {
		animation: todo .5s linear;
	}

	.todo-leave-active {
		animation: todo .5s linear reverse;
	}

	@keyframes todo {
		from{
			opacity: 0%;
		}
		to{
			opacity: 100%;
		}
	}
</style>