<template>
  <div
    class="select"
    :class="{ 'select_dirty' : dirty, 'select_focused' : focused }"
    @click="showMenu()"
  >
    <label class="select__label">{{ label }}</label>
    <div class="select__selected-items">{{ selected_item_text }}</div>
    <input readonly>

    <transition
      enter-active-class="select_enter"
      leave-active-class="select_leave"
    >
      <div class="select__menu" v-show="focused">
        <div class="select__window">
          <div class="select__list-items">
            <div class="select__item"
                 v-for="(item, index) in items"
                 :key="index"
                 @click.stop="toggleSelectedItem(item)"
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
          }
      },
      data() {
          return {
              focused: false,
          }
      },
      /**по-хорошему, нужно припотеть по поводу перевода данных в нужный тип, например, v-model (value) в массив, если в пропсы передан :multiple="true" и тд,
       * но шото лень (простите). Да и зачем это делать, если есть куча уже готовых технологий для этого*/
      methods: {
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
          showMenu() {
              this.focused = true;
              document.addEventListener('click', this.checkClick);
          },
          checkClick() {
              for (const i in this.$el.children) {
                  let el = this.$el.children[i];
                  if (el == event.target) return;
              }
              this.hideMenu();
          },
          hideMenu() {
              this.focused = false;
              document.removeEventListener('click', this.checkClick);
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
    height: 56px;
    border: solid 1px #c4c4c4;
    border-radius: 6px;
    display: flex;
    flex-direction: row;
    align-items: center;
    position: relative;
    .select__selected-items {
      z-index: 1;
      position: absolute;
      padding: 18px 0px 0px 12px;
      font-size: 18px;
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
    border-color: #8a8a8a;
  }
  .select_dirty {
    label {
      transform: translateY(-14px);
      font-size: 14px;
    }
  }
</style>
