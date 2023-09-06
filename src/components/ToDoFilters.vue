<template>
  <div>
    <span v-for="filter in filters"
      @click="setFilter(filter.name)"
      class="mr-2"
      :key="filter.name"
      :style="{ color: activeFilter == filter.name ? '#ccc' : '#fff', cursor: 'pointer' }"
    >{{ filter.label }}</span>
  </div>
</template>

<script>
export default {
  props: {
    activeFilter: String, // Текущий активный фильтр
  },
    data() {
    return {
      filters: [
        {
          name: 'all',
          label: 'Все'
        },
        {
          name: 'completed',
          label: 'Выполненные'
        },
        {
          name: 'uncompleted',
          label: 'Невыполненные'
        },
      ]
    };
  },
  methods: {
    setFilter(filter) {
      // Вызываем событие и передаем выбранный фильтр в родительский компонент
      this.$emit('setActive', filter);
    },
  },
};
</script>