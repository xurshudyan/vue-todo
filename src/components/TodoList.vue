<template>
  <div>
    <input type="text" class="todo-input" placeholder="What needs to be done" v-model="newTodo"
           @keyup.enter="addTodo">
    <transition-group name="fade" enter-active-class="animated fadeInUp" leave-active-class="animated fadeOutDown">
      <todo-item v-for="todo in todosFiltered" :key="todo.id" :todo="todo" :checkAll="!anyRemaining"
      >
      </todo-item>
    </transition-group>

    <div class="extra-container">
      <div><label><input type="checkbox" :checked="!anyRemaining" @change="checkAllTodos"> Check All</label></div>
      <div>{{ remaining }} items left</div>
    </div>

    <div class="extra-container">
      <div>
        <button :class="{ active: filter === 'all' }" @click="changeFilter('all')">All</button>
        <button :class="{ active: filter === 'active' }" @click="changeFilter('active')">Active</button>
        <button :class="{ active: filter === 'completed' }" @click="changeFilter('completed')">Completed
        </button>
      </div>

      <div>
        <transition name="fade">
          <button v-if="showClearCompletedButton" @click="clearCompleted">Clear Completed</button>
        </transition>
      </div>

    </div>
  </div>
</template>

<script>
import {mapMutations, mapGetters} from 'vuex'
import TodoItem from './TodoItem'

export default {
  name: 'todo-list',
  components: {
    TodoItem,
  },
  data() {
    return {
      newTodo: '',
      idForTodo: 3,
    }
  },
  computed: {
    ...mapGetters([
      'remaining',
      'anyRemaining',
      "todosFiltered",
      "showClearCompletedButton"
    ]),
    filter() {
      return this.$store.state.filter
    }
  },
  methods: {
    ...mapMutations([
      "checkAllTodos",
      "clearCompleted",
      "changeFilter"
    ]),
    addTodo() {
      if (this.newTodo.trim()) {
        this.$store.commit('addTodo', {
          id: this.idForTodo,
          title: this.newTodo,
          completed: false,
          editing: false
        })
      }
      this.newTodo = ''
      this.idForTodo++
    }
  }
}
</script>

<style>
@import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.5.2/animate.min.css");

.todo-input {
  width: 100%;
  padding: 10px 18px;
  font-size: 18px;
  margin-bottom: 16px;
}

.todo-input:focus {
  outline: 0;
}

.todo-item {
  margin-bottom: 12px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  animation-duration: 0.3s;
}

.remove-item {
  cursor: pointer;
  margin-left: 14px;
}

.remove-item:hover {
  color: black;
}

.todo-item-left {
  display: flex;
  align-items: center;
}

.todo-item-label {
  padding: 10px;
  border: 1px solid white;
  margin-left: 12px;
}

.todo-item-edit {
  font-size: 24px;
  color: #2c3e50;
  margin-left: 12px;
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
}

.todo-item-edit:focus {
  outline: none;
}

.completed {
  text-decoration: line-through;
  color: grey;
}

.extra-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size: 16px;
  border-top: 1px solid lightgrey;
  padding-top: 14px;
  margin-bottom: 14px;
}

button {
  font-size: 14px;
  background-color: white;
  appearance: none;
  border: none;
  outline: none;
}

button:hover {
  background: lightgreen;
}

button:focus {
  outline: none;
}

.active {
  background: lightgreen;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity .2s;
}

.fade-enter, .fade-leave-to {
  opacity: 0;
}
</style>