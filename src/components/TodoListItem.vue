<template>
  <li>
    <label>
      <input type="checkbox" :checked="todo.done" @change="changeTodo(todo.id)"/>
      <span v-if="!todo.isEdit">{{ todo.value }}</span>
      <input 
        v-else 
        type="text" 
        :value="todo.value" 
        @blur="confirmEdit($event, todo.id)" 
        ref="editInput"
      >
    </label>
    <button class="btn btn-danger" @click="deleteTodo(todo.id)">删除</button>
    <button v-show="!todo.isEdit" class="btn btn-edit" @click="handleEdit(todo.id)">编辑</button>
  </li>
</template>
<script>
export default {
  name: 'TodoListItem',
  props: ['todo'],
  methods:{
    changeTodo(id){
      this.$bus.$emit('checkTodo', id)
    },
    deleteTodo(id){
      if(confirm('确定取消吗？')){
        this.$bus.$emit('removeTodo', id)
      }
    },
    handleEdit(id){
      this.$bus.$emit('handleEdit', id)
      // input框自动获取焦点
      this.$nextTick(() => {
        this.$refs.editInput.focus()
      })
    },
    confirmEdit(e, id){
      const { value } = e.target
      if(!value.trim()){
        alert('内容不能为空')
        return
      }
      this.$bus.$emit('confirmEdit', id, value)
    }
  }
}
</script>
<style scoped>
  /*item*/
  li {
    list-style: none;
    height: 36px;
    line-height: 36px;
    padding: 0 5px;
    border-bottom: 1px solid #ddd;
  }

  li label {
    float: left;
    cursor: pointer;
  }

  li label li input {
    vertical-align: middle;
    margin-right: 6px;
    position: relative;
    top: -1px;
  }

  li button {
    float: right;
    display: none;
    margin-top: 3px;
  }

  li:before {
    content: initial;
  }

  li:last-child {
    border-bottom: none;
  }

  li:hover {
    background: #ddd;
  }

  li:hover button {
    display: block;
  }

</style>