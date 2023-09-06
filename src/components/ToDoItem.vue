<template>
<div>
  <b-card class="todo-item" :class="{ 'complete': editedToDo.complete }">
    <b-row class="align-items-center">
      <b-col>
        <b-card-title>{{ toDo.title }}</b-card-title>
      </b-col>
      <b-col>
        <b-card-text class="text-right">{{ toDo.date }}</b-card-text>
      </b-col>
      <b-col cols="auto" class="ml-auto">
        <div>
          <b-icon-pencil v-b-modal="'editModal_' + toDo.id"></b-icon-pencil>
          <b-icon-trash v-b-modal="'deleteModal_' + toDo.id"></b-icon-trash>
          <b-icon-check @click="toggleComplete"></b-icon-check>
        </div>
      </b-col>
    </b-row>
    <!-- Модальное окно для редактирования задачи -->
    <AddToDoModal :id="editModalId" :value="editedToDo" @ok="saveEditedToDo" :toDo="toDo" @editToDo="saveEditedToDo"></AddToDoModal>
    <!-- Модальное окно для подтверждения удаления задачи -->
    <b-modal :id="deleteModalId" title="Удалить задачу" @ok="deleteToDo" :ok-variant="'outline-secondary'">
      Вы уверены, что хотите удалить задачу "{{ toDo.title }}"?
    </b-modal>
  </b-card>
</div>


</template>

<script>
import { BIconPencil, BIconTrash, BIconCheck } from 'bootstrap-vue'; // Импорт иконок из Bootstrap Vue
import AddToDoModal from '@/components/modal/AddToDoModal.vue';

export default {
  props: {
    toDo: Object, // Принимаем задачу как свойство
  },
    components: {
    BIconPencil,
    BIconTrash,
    BIconCheck,
    AddToDoModal
  },
  data() {
    return {
      editedToDo: {}, // Копия задачи для редактирования
    };
  },
  watch: {
    toDo(newVal){
      //Обновляем объект при изменении props
      this.editedToDo = JSON.parse(JSON.stringify(newVal))
    }
  },
  mounted(){
    //Глубокая копия объекта при монтировании компонента
    this.editedToDo = JSON.parse(JSON.stringify(this.toDo))
  },
  methods: {
    toggleComplete() {
      // Изменяем значение complete на противоположное, вызываем обновление компонента
      this.$set(this.editedToDo, 'complete', !this.editedToDo.complete);
        // Передаем значение в родительский компонент
      this.$emit('toggleComplete', this.editedToDo);
    },
    saveEditedToDo(toDo) {
      // Обновляем задачу после редактирования
      this.editedToDo = toDo
      this.$emit('editToDo', this.editedToDo);
    },
    deleteToDo() {
      // Передаем удаленный элемент в родительский компонент
      this.$emit('deleteToDo', this.editedToDo);
    },
  },
    computed: {
      // Динамически вычисляем значения id модальных окон
      editModalId() {
        return `editModal_${this.toDo.id}`;
      },
      deleteModalId() {
        return `deleteModal_${this.toDo.id}`;
      },
  },
};
</script>

<style scoped>
.todo-item {
  margin: 20px auto;
  background: #fff;
  border-radius: 5px;
  box-shadow: 0 0px 5px 4px #ea4f30;
  height: 80px;
  width: 100%;
  display: flex;
  color: #6c757d
}

.todo-item.complete {
  box-shadow: 0 0px 5px 4px rgb(43, 206, 135);
}

.ml-auto {
  margin-left: auto; /* Перемещение элементов в конец контейнера */
  display: flex;
  align-items: center;
}

.ml-auto svg {
  width: 25px;
  height: 25px;
  margin-right: 10px;
  cursor: pointer;
  color: #6c757d
}


</style>