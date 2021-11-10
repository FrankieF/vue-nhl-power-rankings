<template>
    <header>
        <h1>{{title}}</h1>
    </header>
</template>

<script>
import Button from './Button.vue'

export default {
  name: 'Header',
  props:{
    title: String,
    showAddTask: Boolean
  },
  components: {
      Button
  },
  computed: {
      homePage() {
          if(this.$route.path === '/') {
              return true;
          } else {
              return false;
          }
      }
  }
}
</script>

<style scoped>
    header {
        display: flex;
        justify-content: center;
        margin-bottom: 20px;
    }
</style>
