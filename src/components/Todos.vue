<template>
    <section class="todoapp">
        <header class="header">
            <h1>Todos</h1>
            <input type="text" class="new-todo" placeholder="Ajoutez une tâche" v-model="newTodo" @keyup.enter="addTodo">
        </header>
        <div class="main">
            <input id="toggle-all" type="checkbox" class="toggle-all" v-model="allDone">
            <label for="toggle-all">Mark all as complete</label>
            <ul class="todo-list">
                <li class="todo" v-for="todo in filteredTodos" v-bind:key="todo.name" :class=" {completed : todo.completed, editing: editing === todo}">
                    <div class="view">
                        <input type="checkbox" class="toggle" v-model="todo.completed">
                        <label @dblclick="todoEdit(todo)">{{todo.name}}</label>
                        <button class="destroy" @click.prevent="deleteTodo(todo)"></button>
                    </div>
                    <input type="text" class="edit" v-model="todo.name" @keyup.enter="editDone" @blur="editDone"  v-focus="editing === todo" @keyup.esc="editCancel">
                </li>
            </ul>
        </div>
        <footer class="footer" v-show="todos.length > 0">
            <span class="todo-count"><strong> {{remaining}} </strong> tâches à faire </span>
            <ul class="filters">
                <li><a href="#" :class=" {selected: filter === 'all'}" @click.prevent="filter = 'all'">Toutes</a></li>
                <li><a href="#" :class=" {selected: filter === 'todo'}" @click.prevent="filter = 'todo'">A faire</a></li>
                <li><a href="#" :class=" {selected: filter === 'done'}" @click.prevent="filter = 'done'">Faites</a></li>
            </ul>
            <button v-show="completed" @click.prevent="deleteCompleted" class="clear-completed">Supprimer tâches terminé</button>
        </footer>
    </section>
</template>

<script>
export default {
  data () {
    return {
      todos: [{
        name: 'Nouvelle tâche',
        completed: false
      }],
      newTodo: '',
      filter: 'all',
      editing: null,
      oldTodo: ''
    }
  },

  methods: {
    addTodo () {
      this.todos.push({
        completed: false,
        name: this.newTodo
      })
      this.newTodo = ''
    },

    deleteTodo (todo) {
      this.todos = this.todos.filter(i => i !== todo)
    },

    deleteCompleted () {
      this.todos = this.todos.filter(todo => !todo.completed)
    },

    todoEdit (todo) {
      this.oldTodo = todo.name
      this.editing = todo
    },

    editDone () {
      this.editing = null
    },

    editCancel () {
      this.editing.name = this.oldTodo
      this.editDone()
    }
  },

  computed: {
    allDone: {
      get () {
        return this.remaining === 0
      },

      set (value) {
        this.todos.forEach(todo => {
          todo.completed = value
        })
      }
    },

    remaining () {
      return this.todos.filter(todo => !todo.completed).length
    },

    completed () {
      return this.todos.filter(todo => todo.completed).length
    },

    filteredTodos () {
      if (this.filter === 'todo') {
        return this.todos.filter(todo => !todo.completed)
      } else if (this.filter === 'done') {
        return this.todos.filter(todo => todo.completed)
      }

      return this.todos
    }
  },

  directives: {
    focus (el, value) {
      if (value) {
        Vue.nextTick(() => {
          el.focus()
        })
      }
    }
  }
}
</script>

<style src="./todos.css"></style>
