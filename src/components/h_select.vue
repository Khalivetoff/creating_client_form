<template>
  <div
    class="select"
    :class="{ 'select_dirty' : dirty, 'select_focused' : focused, 'select_error' : notification }"
    @mouseup="showMenu()"
  >
    <div class="select__main-elements">
      <label ref="label" class="select__label">{{ label }}</label>
      <div ref="selected_area" class="select__selected-items">{{ selected_item_text }}</div>
      <input ref="input" readonly>
      <span ref="icon" class="select__required-icon" v-if="support_block">*</span>
    </div>
    <div class="select__info" v-if="support_block">{{ notification }}</div>
    <transition
      enter-active-class="select_enter"
      leave-active-class="select_leave"
    >
      <div class="select__menu" v-show="focused">
        <div class="select__window">
          <div class="select__list-items" ref="select_list">
            <div class="select__item"
                 v-for="(item, index) in items"
                 :key="index"
                 @mouseup.stop="toggleSelectedItem(item)"
                 :class="{ 'select__item_selected' : checkSelected(item) }"
            >{{ item[item_text] !== undefined ? item[item_text] : item }}</div>
          </div>
        </div>
      </div>
    </transition>

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
          value: {
              default() {
                  return '';
              }
          },
          multiple: {
              type: Boolean,
              default() {
                  return false;
              }
          },
          item_text: {
              type: String,
              default() {
                  return 'Name';
              }
          },
          item_value: {
              type: String,
              default() {
                  return 'Id';
              }
          },
          items: {
              type: Array,
              default() {
                  return [];
              }
          },
          return_object: {
              type: Boolean,
              default() {
                  return false;
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
              focused: false,
          }
      },
      methods: {
          showMenu() {
              this.focused = true;
              document.addEventListener('mouseup', this.checkClick);
          },
          checkClick() {
              if (event.target == this.$refs.input || event.target == this.$refs.label ||
                  event.target == this.$refs.selected_area || event.target == this.$refs.icon) return;
              this.hideMenu();
          },
          hideMenu() {
              this.focused = false;
              document.removeEventListener('mouseup', this.checkClick);
          },
          toggleSelectedItem(item) {
              if (!this.multiple) {
                  if (!this.return_object) {
                      if (item[this.item_value]) {
                          this.$emit('input', item[this.item_value]);
                      } else {
                          this.$emit('input', item);
                      }
                  } else {
                      this.$emit('input', item);
                  }
                  this.hideMenu();
              } else {
                  if (!this.return_object) {
                      if (item[this.item_value]) {
                          if (this.value.includes(item[this.item_value])) {
                              this.$emit('input', this.value.filter(i => i !== item[this.item_value]));
                          } else {
                              this.$emit('input', this.value.concat(item[this.item_value]));
                          }
                      } else {
                          if (this.value.includes(item)) {
                              this.$emit('input', this.value.filter(i => i !== item));
                          } else {
                              this.$emit('input', this.value.concat(item));
                          }
                      }
                  } else {
                      if (item[this.item_value]) {
                          if (this.value.map(i => i[this.item_value]).includes(item[this.item_value])) {
                              this.$emit('input', this.value.filter(i => i[this.item_value] !== item[this.item_value]));
                          } else {
                              this.$emit('input', this.value.concat(item));
                          }
                      } else {
                          if (this.value.includes(item[this.item_value])) {
                              this.$emit('input', this.value.filter(i => i[this.item_value] !== item[this.item_value]));
                          } else {
                              this.$emit('input', this.value.concat(item));
                          }
                      }
                  }
              }
          },

          checkSelected(item) {
              if (!this.multiple) {
                  if (!this.return_object) {
                      if (item[this.item_value]) {
                          if (this.value == item[this.item_value]) return true;
                          return false;
                      } else {
                          if (this.value == item) return true;
                          return false;
                      }
                  } else {
                      if (item[this.item_value]) {
                          if (this.value[this.item_value] == item[this.item_value]) return true;
                          return false;
                      } else {
                          if (this.value[this.item_value] == item) return true;
                          return false;
                      }
                  }
              } else {
                  if (!this.return_object) {
                      if (item[this.item_value]) {
                          if (this.value.includes(item[this.item_value])) return true;
                          return false;
                      } else {
                          if (this.value.includes(item)) return true;
                          return false;
                      }
                  } else {
                      if (item[this.item_value]) {
                          if (this.value.map(i => i[this.item_value]).includes(item[this.item_value])) return true;
                          return false;
                      } else {
                          if (this.value.map(i => i[this.item_value]).includes(item)) return true;
                          return false;
                      }
                  }
              }
          }
      },
      computed: {
          selected_item_text() {
              if (!this.multiple) {
                  let sel_item;
                  if (!this.return_object) {
                      if (typeof this.items[0] !== 'object') {
                          sel_item = this.items.filter(i => i == this.value)[0];
                      } else {
                          sel_item = this.items.filter(i => i[this.item_value] == this.value)[0];
                      }
                  } else {
                      sel_item = this.items.filter(i => i[this.item_value] == this.value[this.item_value])[0];
                  }
                  if (typeof this.items[0] !== 'object') {
                      return sel_item ? sel_item : '';
                  } else {
                      return sel_item ? sel_item[this.item_text] : '';
                  }
              } else {
                  if (this.value.length && this.value.length === this.items.length) {
                      return 'Все';
                  } else if (this.value.length > 1 && this.value.length < this.items.length) {
                      return 'Несколько';
                  } else if (this.value.length) {
                      if (!this.return_object) {
                          return this.value[0];
                      } else {
                          return this.value[0][this.item_text];
                      }
                  } else {
                      return '';
                  }
              }
          },
          dirty() {
              if (!this.multiple) {
                  if (this.value !== '' && this.value !== null) return true;
                  return false;
              } else {
                  if (this.value.length) return true;
                  return false;
              }
          }
      }
  }
</script>

<style lang="scss">
  .select_enter {
    animation: ENTER 150ms ease-in-out;
  }
  @keyframes ENTER {
    from {
      top: 50px;
      opacity: 0;
    }
    to {
      top: 56px;
      opacity: 1;
    }
  }
  .select_leave {
    animation: LEAVE 150ms ease-in-out;
  }
  @keyframes LEAVE {
    from {
      top: 56px;
      opacity: 1;
    }
    to {
      top: 50px;
      opacity: 0;
    }
  }
  .select {
    position: relative;
    .select__info {
      font-size: 12px;
      color: #f00;
      padding: 3px 0px 0px 12px;
      min-height: 17px;
    }
    .select__main-elements {
      position: relative;
      height: 56px;
      border: solid 2px #c4c4c4;
      transition: 200ms;
      border-radius: 6px;
      display: flex;
      flex-direction: row;
      align-items: center;
      .select__required-icon {
        position: absolute;
        right: 6px;
        top: 3px;
        height: 15px;
        font-size: 18px;
        color: #999;
      }
      .select__selected-items {
        position: absolute;
        padding: 27px 0px 0px 12px;
        font-size: 16px;
        width: 100%;
        height: 56px;
        color: #3d3d3d;
        white-space: nowrap;
        overflow-x: hidden;
      }
      label {
        z-index: 1;
        font-size: 18px;
        padding: 0px 0px 0px 12px;
        transition: 200ms;
        color: #999;
      }
      input {
        position: absolute;
        height: 100%;
        width: 100%;
        border: none;
        outline: none;
        padding: 0px 0px 0px 12px;
        border-radius: 6px;
        z-index: 1;
        background: transparent;
      }
    }

    .select__menu {
      position: absolute;
      top: 56px;
      width: 100%;
      box-shadow: rgba(0, 0, 0, 0.12) 0px 0px 9px;
      border-radius: 6px;
      padding: 6px 0px;
      z-index: 5;
      background: #fff;
      .select__window {
        max-height: 155px;
        overflow: auto;
        .select__list-items {
          .select__item {
            padding: 6px 12px;
            cursor: pointer;
            user-select: none;
            color: #3d3d3d;
          }
          .select__item:hover {
            background: #fafafa;
          }
          .select__item_selected {
            color: #1097a1;
          }
        }
      }
    }
  }
  .select_focused {
    .select__main-elements {
      border-color: #8a8a8a;
    }
  }
  .select_error {
    .select__main-elements {
      border-color: #f00;
      label {
        color: #f00;
      }
      .select__required-icon {
        color: #f00;
      }
    }
  }
  .select_dirty {
    .select__main-elements {
      label {
        transform: translateY(-12px);
        font-size: 13px;
      }
    }
  }
</style>
