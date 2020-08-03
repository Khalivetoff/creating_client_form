<template>
  <div
    class="text-field"
    :class="{ 'text-field_dirty' : (value !== '' && value !== null), 'text-field_focused' : focused }">
    <label class="text-field__label">{{ label }}</label>
    <input
      :type="type"
      :value="value"
      @input="$emit('input', $event.target.value)"
      @focusin="focused=true"
      @focusout="focused=false"
    >
  </div>
</template>

<script>
  export default {
      props: {
          label: {
              default() {
                  return '';
              }
          },
          type: {
              type: String,
              default() {
                  return 'text';
              }
          },
          value: {
              default() {
                  return '';
              }
          }
      },
      data() {
          return {
              focused: false
          }
      }
  }
</script>

<style lang="scss">
  .text-field {
    border: solid 1px #c4c4c4;
    border-radius: 6px;
    display: flex;
    flex-direction: row;
    align-items: center;
    height: 56px;
    position: relative;
    label {
      z-index: 1;
      font-size: 18px;
      padding: 0px 0px 0px 12px;
      transition: 200ms;
      color: #999;
    }
    input {
      padding: 18px 0px 0px 12px;
      position: absolute;
      height: 100%;
      width: 100%;
      outline: none;
      border: none;
      font-size: 18px;
      border-radius: 6px;
      z-index: 2;
      background: transparent;
    }
  }
  .text-field_focused {
    border-color: #8a8a8a;
    label {
      transform: translateY(-14px);
      font-size: 14px;
    }
  }
  .text-field_dirty {
    label {
      transform: translateY(-14px);
      font-size: 14px;
    }
  }
</style>
