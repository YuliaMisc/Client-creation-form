<template>
  <div id="app">
    <Layout/>
    <AddClient />
  </div>
</template>

<script>
  import Layout from './components/Layout.vue';
  import AddClient from './pages/AddClient.vue';

  export default {
    name: 'App',
    components: { Layout, AddClient },
  }
</script>


