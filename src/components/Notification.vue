<template>
  <div class="notification" v-if="notificationIsShow">
    <div class="notificotion__tiile">Новый клиент успешно создан</div>
    <div class="progress progress-infinite">
      <div class="progress-bar3"></div>
    </div> 
  </div>
</template>

<script>
  export default {
      name: "NotificationForm",
      data() {
          return {
            notificationIsShow: false,
          }
      },
      methods: {
        hideNotification () {
          setTimeout(() => {
            this.notificationIsShow = false
          }, 5000);
        },
      },
      mounted () {
      this.hideNotification()
    },
  }
</script>