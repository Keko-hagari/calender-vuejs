<template>
  <div id="app">
    <Layout />
    <Loading v-if='loading' />
  </div>
</template>

<script>
  import Layout from './Layout.vue'
  import Loading from './Loading.vue'
  export default {
    components:{
      Layout,
      Loading
    },
    data() {
      return{
        loading: false
      }
    },
    mounted() {
      setTimeout(() => {
        this.loading = false
      }, 3000)
    },
  }
</script>