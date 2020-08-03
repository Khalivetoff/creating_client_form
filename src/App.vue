<template>
  <div id="app">
    <creating-client></creating-client>
  </div>
</template>

<script>
    import CreatingClientComponent from './components/creating_client_form.vue'


    export default {
        data() {
            return {}
        },
        name: 'App',
        components: {
            'creating-client': CreatingClientComponent
        }
    }
</script>

<style>
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
