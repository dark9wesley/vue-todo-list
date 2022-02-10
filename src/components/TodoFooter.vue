<template>
  <div class="todo-footer" v-show="total">
    <label>
      <input type="checkbox" v-model="isAll" />
    </label>
    <span>
      <span>已完成{{doneTotal}}</span> / 全部{{total}}
    </span>
    <button class="btn btn-danger" @click="clearCheckedTodo">清除已完成任务</button>
  </div>
</template>
<script>
export default {
  name: 'TodoFooter',
  props: ["todos"],
  computed: {
    total(){
      return this.todos.length
    },
    doneTotal(){
      return this.todos.reduce((pre, todo) => todo.done ? pre + 1 : pre, 0)
    },
    isAll: {
      get(){
        return this.doneTotal === this.total && this.total !== 0
      },
      set(done){
        this.$emit('checkAllTodo', done)
      }
    }
  },
  methods: {
    clearCheckedTodo(){
      if(confirm('确定清除已完成任务吗？')){
        this.$emit('removeCheckedTodo')
      }
    }
  }
}
</script>
<style scoped>
   /*footer*/
  .todo-footer {
    height: 40px;
    line-height: 40px;
    padding-left: 6px;
    margin-top: 5px;
  }

  .todo-footer label {
    display: inline-block;
    margin-right: 20px;
    cursor: pointer;
  }

  .todo-footer label input {
    position: relative;
    top: -1px;
    vertical-align: middle;
    margin-right: 5px;
  }

  .todo-footer button {
    float: right;
    margin-top: 5px;
  }
</style>