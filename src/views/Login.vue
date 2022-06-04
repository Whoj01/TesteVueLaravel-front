<template>
  <div class="home">
   <LoginForm/>
  </div>
</template>

<script>
// @ is an alias to /src
  import LoginForm from '../components/login.vue'
  export default {
    name: 'homeLogin',
    components: {
      LoginForm
    }
  }
</script>


