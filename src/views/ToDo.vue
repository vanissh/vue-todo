<template>
  <div class="container">
    <div class="d-flex w-100 flex-row justify-content-between">
      <h1>ToDo List</h1>
      <b-button variant="light" @click="openModal">Добавить задачу</b-button>
    </div>
    <ToDoFilters @setActive="setActive" :activeFilter="activeFilter" :v-model="todos"></ToDoFilters>
    <AddToDoModal ref="addTaskModal" @addToDo="addToDo"></AddToDoModal>
    <ToDoList :value="filteredTodos" @deleteToDo="deleteToDo" @toggleComplete="editToDo" @editToDo="editToDo"></ToDoList>
  </div>
</template>

<script>
import AddToDoModal from '@/components/modal/AddToDoModal.vue'; // Импортируйте ваш компонент
import ToDoList from '@/components/ToDoList.vue';
import ToDoFilters from '@/components/ToDoFilters.vue';

export default {
  components: {
    AddToDoModal,
    ToDoList,
    ToDoFilters
  },
  data() {
    return {
      todos: [], // Здесь будут храниться задачи
      activeFilter: 'all',
      taskIdCounter: 0,
    };
  },
  created(){
    // Получение текущего списка задач из Local Storage
    this.todos = JSON.parse(localStorage.getItem('todos')) || [];

    // Установка счетчика taskIdCounter
    const maxId = this.todos.reduce((max, task) => (task.id > max ? task.id : max), 0);
    this.taskIdCounter = maxId + 1;
  },
  methods: {
    openModal() {
      this.$refs.addTaskModal.openModal();
    },
    setActive(val){
      this.activeFilter = val
    },
    addToDo(newToDo) {
      // Добавление новой задачи в список
      newToDo.id = this.taskIdCounter
      this.taskIdCounter++
      this.todos.push(newToDo);

      // Обновление списка задач в Local Storage
      localStorage.setItem('todos', JSON.stringify(this.todos));
    },
    deleteToDo(toDo){
      const indexToDelete = this.todos.findIndex(task => task.id == toDo.id);
      if (indexToDelete !== -1) {
        // Удаляем задачу по ее индексу в массиве задач
        this.todos.splice(indexToDelete, 1);

        // Обновляем список задач в Local Storage
        localStorage.setItem('todos', JSON.stringify(this.todos));
      }
    },
    editToDo(toDo){
      const indexToUpdate = this.todos.findIndex(t => t.id === toDo.id);
      if (indexToUpdate !== -1) {
        // Обновляем задачу в списке, используем $set, чтобы изменения ослеживались
        this.$set(this.todos, indexToUpdate, toDo);

        // Обновляем список задач в Local Storage
        localStorage.setItem('todos', JSON.stringify(this.todos));
      }
    },
  },
  computed: {
    filteredTodos(){
      // Фильтрация задач на основании выбранных фильтров
      return this.activeFilter == 'completed' ? this.todos.filter(x => x.complete == true) :
            this.activeFilter == 'uncompleted' ? this.todos.filter(x => x.complete != true) :
            this.todos

    }
  }
};
</script>
<style scoped>

  .container {
    max-width: 800px;
    margin: 30px auto;
    color: #fff;
  }
</style>