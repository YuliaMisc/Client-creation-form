<template>
  <div class="wrapper">
    <div class="header">
      <div class="header__container _container">
      <div class="header__title">
        <div class="header__title__logo">Medicare</div>
        <div class="header__title__avatar _ibg">
          <img src="../assets/image.jpg" alt="">
        </div>
      </div>
    </div>
    </div>
    <div class="bread-crumbs">
      <div class="bread-crumbs__container _container">
        <div class="bread-crumbs__content">
          <a href="#">Клиенты  ► </a>
          <span>Добавление нового клиента</span>
        </div>
      </div>
    </div>
    <AddClientPage/>
  </div>
</template>

<script>
import AddClientPage from './AddClientPage.vue';

export default {
  name: 'LayoutPage',
  components: {
    AddClientPage
  }
}
</script>