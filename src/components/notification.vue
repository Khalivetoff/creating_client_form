<template>
  <div class="notification">
    <div class="notification__wrapper">
      <div
        class="notification__item"
        v-for="(item, key) in notifications"
        :key="key"
        @click="deleteByClick(key)"
        :class="{
          'error' : item.type === 'error',
          'success' : item.type === 'success',
          'warning' : item.type === 'warning',
          'info' : item.type === 'info'
        }"
      >
        <div class="notification__container">
          <div v-if="item.title" class="notification__title">{{ item.title }}</div>
          <div v-if="item.text" class="notification__text">{{ item.text }}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
      data() {
          return {
              notifications: {},
          }
      },
      methods: {
          addNotificationItem(item) {
              let key = 0;
              while (this.notifications[key] !== undefined) {
                  key++;
              }
              this.$set(this.notifications, key, {
                  title: item.title,
                  text: item.text,
                  type: item.type
              });
             let counter;
             switch (item.type) {
                case 'error':
                    counter = 5000;
                    break;
                case 'warning':
                    counter = 2000;
                    break;
                case 'info':
                    counter = 3000;
                    break;
                case 'success':
                    counter = 2000;
                    break;
             }
              setTimeout(() => {
                  this.$delete(this.notifications, key);
              }, counter)
          },
          deleteByClick(key) {
              this.$delete(this.notifications, key);
          }
      },
      created() {
          this.$root.$on('add-item-in-notifications', this.addNotificationItem); //это некруто, но во Vuex не вижу надобности
      }
  }
</script>

<style lang="scss">
  .notification {
    position: absolute;
    right: 0;
    bottom: 0;
    .notification__wrapper {
      display: flex;
      flex-direction: column;
      .notification__item {
        cursor: pointer;
        user-select: none;
        margin: 6px 12px 12px 12px;
        padding: 16px;
        border-radius: 6px;
        .notification__container {
          display: flex;
          flex-direction: column;
          align-items: start;
          justify-content: start;
          min-width: 320px;
          max-width: 320px;
          color: #fff;
          .notification__title {
            font-size: 26px;
          }
          .notification__text {
            font-size: 18px;
          }
        }
      }
      .notification__item.error {
        background: #ff00d5e6;
      }
      .notification__item.success {
        background: #00bf70d1;
      }
      .notification__item.warning {
        background: #ffe500e6;
      }
      .notification__item.info {
        background: #00c4ffe6;
      }
    }
  }

</style>
