<template>
  <div id="app">
    <ToDo/>
  </div>
</template>
<script>
  import ToDo from './views/ToDo.vue'

  export default{
    components:{
      ToDo,
    }
  }

</script>

<style >
  @import 'assets/index.css';
</style>