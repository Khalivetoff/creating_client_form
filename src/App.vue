<template>
  <div id="app">
    <div class="card-wrapper">
      <div class="card-form">
        <creating-client></creating-client>
      </div>
    </div>
    <notification></notification>
  </div>
</template>

<script>
    import CreatingClientComponent from './components/creating_client_form.vue'
    import Notification from './components/notification.vue'

    export default {
        data() {
            return {}
        },
        name: 'App',
        components: {
            'creating-client': CreatingClientComponent,
            'notification': Notification,

        },
        mounted() {
            console.log(this);
        }
    }
</script>

<style lang="scss">
  .card-wrapper {
    height: 100vh;
    width: 100vw;
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    .card-form {
      max-height: calc(100vh - 50px);
      overflow: auto;
      max-width: 950px;
      box-shadow: rgba(0, 0, 0, 0.12) 0px 0px 9px;
      border-radius: 6px;
    }
  }
  @font-face {
    font-family: "Roboto";
    src: url('./fonts/Roboto/Roboto-Medium.ttf'); /* IE9 Compat Modes */
    src: url('./fonts/Roboto/Roboto-Medium.ttf') format('embedded-opentype'), /* IE6-IE8 */
    url('./fonts/Roboto/Roboto-Medium.ttf') format('woff'), /* Pretty Modern Browsers */
    url('./fonts/Roboto/Roboto-Medium.ttf')  format('truetype'), /* Safari, Android, iOS */
    //  url('./fonts/Roboto/Roboto-Medium.ttf') format('svg'); /* Legacy iOS */
  }

  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: Roboto;
  }
</style>
