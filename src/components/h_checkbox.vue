<template>
  <div class="checkbox">
    <label>{{ label }}</label>
    <input type="checkbox" :value="value" @input="$emit('input', $event.target.value)">
  </div>
</template>
<script>
  export default {
      props: ['value', 'label']
  }
</script>

<style lang="scss">
  .checkbox {
    display: flex;
    flex-direction: row;
    align-items: center;
    label {
      color: red;
      font-size: 16px;
    }
  }
</style>
