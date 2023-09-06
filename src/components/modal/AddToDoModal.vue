<template>
  <b-modal :id="id" v-model="showModal" @hidden="resetForm" @ok="addToDo" :title="toDo ? 'Редактировать задачу' : 'Добавить задачу'" @cancel="resetForm" @show="updateTask">
    <form @submit.prevent="addTask" class="todo-form">
      <b-form-group label="Наименование:" label-for="title">
        <b-form-input v-model="task.title" id="title" maxlength="25" required></b-form-input>
      </b-form-group>
      <b-form-group label="Описание:" label-for="description">
        <b-form-textarea v-model="task.description" id="description" required></b-form-textarea>
      </b-form-group>
      <b-form-group label="Дата:">
        <b-form-datepicker v-model="task.date" format="yyyy-MM-dd" required></b-form-datepicker>
      </b-form-group>
    </form>
    <template slot="modal-footer">
      <b-button variant="outline-secondary" @click="toDo ? editToDo() : addToDo()">{{toDo ? 'Сохранить' : 'Добавить'}}</b-button>
      <b-button variant="secondary" @click="resetForm();showModal = false;">Отменить</b-button>
    </template>
  </b-modal>
</template>

<script>
export default {
  data() {
    return {
      showModal: false,
      task: {
        title: '',
        description: '',
        date: null,
        complete: false,
        id: null
      },
    };
  },
  props: {
    id: String,
    toDo: Object
  },
  mounted(){
    if(this.toDo){
      this.task = JSON.parse(JSON.stringify(this.toDo))
    }
  },
  watch: {
    toDo(newVal){
      if(newVal){
        this.task = JSON.parse(JSON.stringify(newVal))
      }
    }
  },
  computed: {
    isFormValid() {
      // Проверка на заполненность обязательных полей
      return !!this.task.title && !!this.task.description && !!this.task.date;
    },
  },
  methods: {
    openModal() {
      this.showModal = true;
    },
    updateTask() {
      // Проверяем, если это режим редактирования и toDo существует
      if (this.toDo) {
        // Копируем данные из this.toDo в this.task
        this.task = JSON.parse(JSON.stringify(this.toDo));
      } else {
        // Если это режим добавления, сбрасываем данные
        this.resetForm();
      }
    },
    addToDo() {
      if (this.isFormValid) {
        this.$emit("addToDo", this.task)
        this.resetForm();
        // Закрываем модальное окно
        this.showModal = false;
      } else {
        // Показать сообщение об ошибке или предупреждение
        alert("Пожалуйста, заполните все обязательные поля!");
      }
    },
    editToDo(){
      // Проверка на заполненность обязательных полей
      if (this.isFormValid) {
        this.$emit("editToDo", this.task)
        this.resetForm();
        this.showModal = false;
      } else {
        // Показать сообщение об ошибке или предупреждение
        alert("Пожалуйста, заполните все обязательные поля!");
      }
    },
    resetForm() {
      // Сбрасываем значения полей
      this.task = {
        title: '',
        description: '',
        date: null,
        complete: false,
        id: null
      }
    },
  },
};
</script>

<style scoped>
  .todo-modal {
    color: #ccc
  }
</style>