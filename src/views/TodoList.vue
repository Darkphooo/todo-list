<template>
  <div class="todo-list">
    <h2>Todo-List</h2>
    <div class="create-area">
      <el-input class="new-task-input" v-model="newTask" placeholder="Add new task here" @keyup.enter="addNewTask"></el-input>
      <el-button type="primary" icon="el-icon-plus" @click="addNewTask"></el-button>
      <el-button type="primary" @click="addNewList">Add New List</el-button>
    </div>
    <div class="list-area">
      <div class="list-item" v-for="(listItem, index) in list" :key="index">
        <h3>{{ listItem.title }}</h3>
        <div class="task" v-for="(item, index) in listItem.task" :key="index">
          <el-checkbox v-model="item.checked">{{ item.title }}</el-checkbox>
        </div>
      </div>
    </div>
    <div class="remove-area">
      <el-button type="primary" icon="el-icon-delete" @click="removeTask">Remove tasks</el-button>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src

export default {
  name: 'todoList',
  data () {
    return {
      newTask: null,
      list: []
    }
  },
  mounted: function () {
    if (localStorage.getItem('todoList')) {
      let storage = localStorage.getItem('todoList')
      this.list = JSON.parse(storage)
    } else {
      let storage = [{
        title: 'List1',
        task: []
      }]
      this.list = storage
      localStorage.setItem('todoList', storage)
    }
  },
  methods: {
    addNewTask: function () {
      if (this.newTask) {
        var current = this.list.length
        var obj = {
          title: this.newTask,
          checked: false
        }
        this.list[current - 1].task.push(obj)
        this.newTask = null
        var storage = JSON.stringify(this.list)
        localStorage.setItem('todoList', storage)
      }
    },
    addNewList: function () {
      var current = this.list.length + 1
      var obj = {
        title: 'List' + current,
        task: []
      }
      this.list.push(obj)
      this.newTask = null
      var storage = JSON.stringify(this.list)
      localStorage.setItem('todoList', storage)
    },
    removeTask: function () {
      var list = this.list
      for (let i in list) {
        list[i].task = list[i].task.filter(task => task.checked === false)
        console.log(list[i].task)
      }
      this.list = list
      var storage = JSON.stringify(this.list)
      localStorage.setItem('todoList', storage)
    }
  }
}
</script>

<style lang="stylus" scoped>
.todo-list
  width 700px
.create-area
  width 700px
  display flex
  flex-direction row
  justify-content flex-start
  align-items center
  margin-bottom 20px
.new-task-input
  width 400px
.list-area
  min-height 200px
  display flex
  flex-direction row
  flex-wrap wrap
  justify-content flex-start
  align-items flex-start
  margin-bottom 20px
.list-item
  width 200px
  margin-right 50px
.list-item:nth-child(3n)
  margin-right 0
</style>
