<template>
  <div class="text-field"
       :class="{ 'text-field_dirty' : (value !== '' && value !== null), 'text-field_focused' : focused, 'text-field_error' : notification }">
    <div class="text-field__main-elements">
      <label
        :class="{ 'active-label' : type === 'date' }"
        class="text-field__label"
      >{{ label }}</label>
      <input
        :type="type"
        :value="value"
        @input="$emit('input', $event.target.value)"
        @focusin="focused=true"
        @focusout="focused=false"
        autocomplete="\|\|\|\|\|\"
      >
      <span class="text-field__required-icon" v-if="support_block">*</span>
    </div>
    <div class="text-field__info" v-if="support_block">{{ notification }}</div>
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
          },
          support_block: {
              type: Boolean,
              default() {
                  return false;
              }
          },
          notification: {
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
    position: relative;
    .text-field__main-elements {
      position: relative;
      border: solid 2px #c4c4c4;
      border-radius: 6px;
      display: flex;
      flex-direction: row;
      align-items: center;
      height: 56px;
      transition: 200ms;
      .text-field__required-icon {
        position: absolute;
        right: 6px;
        top: 3px;
        height: 15px;
        font-size: 18px;
        color: #999;
      }
      label {
        z-index: 1;
        font-size: 18px;
        padding: 0px 0px 0px 12px;
        transition: 200ms;
        color: #999;
      }
      label.active-label {
        transform: translateY(-12px);
        font-size: 13px;
      }
      input {
        padding: 18px 0px 0px 12px;
        position: absolute;
        height: 100%;
        width: 100%;
        outline: none;
        border: none;
        font-size: 16px;
        border-radius: 6px;
        z-index: 2;
        background: transparent;
        color: #3d3d3d;
      }
    }
    .text-field__info {
      font-size: 12px;
      color: #f00;
      padding: 3px 0px 0px 12px;
      min-height: 17px;
    }
  }
  .text-field_focused {
    .text-field__main-elements {
      border-color: #8a8a8a;
      label {
        transform: translateY(-12px);
        font-size: 13px;
      }
    }
  }
  .text-field_error {
    .text-field__main-elements {
      border-color: #f00;
      label {
        color: #f00;
      }
      .text-field__required-icon {
        color: #f00;
      }
    }
  }
  .text-field_dirty {
    .text-field__main-elements {
      label {
        transform: translateY(-12px);
        font-size: 13px;
      }
    }
  }
</style>
