<template>
  <div class="">
    <b-list-group v-if="todos.length">
      <ToDoItem
        v-for="(toDo, index) in todos"
        :key="index"
        :toDo="toDo"
        @deleteToDo="deleteToDo"
        @editToDo="saveEditedToDo"
        @toggleComplete="toggleComplete"
      ></ToDoItem>
    </b-list-group>
    <div v-else class="empty">Здесь будут ваши задачи</div>
  </div>
</template>

<script>
import ToDoItem from '@/components/ToDoItem.vue'; // Импорт компонента ToDoItem

export default {
  components: {
    ToDoItem,
  },
  props: {
    value: Array,
  },
  data() {
    return {
      todos: [], // Здесь будут храниться задачи
    };
  },
  watch: {
    value(newValue){
      this.todos = [...newValue]
    }
  },
  mounted() {
    // Копируем задачи
    this.todos = [...this.value]
  },
  methods: {
    // Передаем события в родительский компонент ToDO
    deleteToDo(editedToDo) {
      this.$emit('deleteToDo', editedToDo);
    },
    toggleComplete(editedToDo) {
      this.$emit('toggleComplete', editedToDo);
    },
    saveEditedToDo(editedToDo) {
      this.$emit('editToDo', editedToDo);
    },
  },
};
</script>

<style scoped>
  .empty {
    width: 100%;
    display: flex;
    justify-content: center;
    margin-top: 30px
  }
</style>

