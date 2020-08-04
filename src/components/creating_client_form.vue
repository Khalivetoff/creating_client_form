<template>
  <div class="validate-form">
    <div class="validate-form__section">
      <div class="validate-form__title">Основная информация</div>
      <div class="validate-form__item">
        <h-text-field
          label="Фамилия"
          v-model="surname"
          support_block
          :notification="validate ? $v.surname.$invalid ? 'Обязательное поле' : '' : null"
        ></h-text-field>
      </div>
      <div class="validate-form__item">
        <h-text-field
          label="Имя"
          v-model="name"
          support_block
          :notification="validate ? $v.name.$invalid ? 'Обязательное поле' : '' : null"
        ></h-text-field>
      </div>
      <div class="validate-form__item">
        <h-text-field
          label="Отчество"
          v-model="second_name"
        ></h-text-field>
      </div>
      <div class="validate-form__item">
        <h-text-field
          label="Дата рождения"
          type="date"
          v-model="birthday"
          support_block
          :notification="validate ? $v.birthday.$invalid ? 'Обязательное поле' : '' : null"
        ></h-text-field>
      </div>
      <div class="validate-form__item">
        <h-text-field
          label="Телефон"
          v-model="phone"
          type="number"
          support_block
          :notification="validate ? $v.phone.$invalid ? 'Начинается с 7, 11 символов' : '' : null"
        ></h-text-field>
      </div>
      <div class="validate-form__item">
        <h-select
          label="Пол"
          v-model="gender"
          :items="gender_arr"
        ></h-select>
      </div>
      <div class="validate-form__item">
        <h-select
          label="Группа клиентов"
          v-model="client_group"
          :items="client_group_arr"
          :multiple="true"
          support_block
          :notification="validate ? $v.client_group.$invalid ? 'Обязательное поле' : '' : null"
        ></h-select>
      </div>
      <div class="validate-form__item">
        <h-select
          label="Лечащий врач"
          v-model="doctor"
          :items="doctor_arr"
        ></h-select>
      </div>
      <div class="validate-form__item validate-form__item_checkbox-wrapper">
        <checkbox
          v-model="send_sms"
          label="Отправлять СМС"
        ></checkbox>
      </div>
    </div>

    <div class="validate-form__section">
      <div class="validate-form__title">Адрес</div>
      <div class="validate-form__item">
        <h-text-field
          label="Индекс"
          type="number"
          v-model="index"
        ></h-text-field>
      </div>
      <div class="validate-form__item">
        <h-text-field
          label="Страна"
          v-model="country"
        ></h-text-field>
      </div>
      <div class="validate-form__item">
        <h-text-field
          label="Область"
          v-model="region"
        ></h-text-field>
      </div>
      <div class="validate-form__item">
        <h-text-field
          label="Город"
          v-model="city"
          support_block
          :notification="validate ? $v.city.$invalid ? 'Обязательное поле' : '' : null"
        ></h-text-field>
      </div>
      <div class="validate-form__item">
        <h-text-field
          label="Улица"
          v-model="street"
        ></h-text-field>
      </div>
      <div class="validate-form__item">
        <h-text-field
          label="Дом"
          v-model="house"
        ></h-text-field>
      </div>
    </div>

    <div class="validate-form__section">
      <div class="validate-form__title">Документы</div>
      <div class="validate-form__item">
        <h-select
          v-model="type_document"
          :items="types_document"
          label="Тип документа"
          support_block
          :notification="validate ? $v.type_document.$invalid ? 'Обязательное поле' : '' : null"
        ></h-select>
      </div>
      <div class="validate-form__item">
        <h-text-field
          v-model="series"
          type="number"
          label="Серия"
        ></h-text-field>
      </div>
      <div class="validate-form__item">
        <h-text-field
          v-model="number"
          type="number"
          label="Номер"
        ></h-text-field>
      </div>
      <div class="validate-form__item">
        <h-text-field
          v-model="ussied"
          label="Кем выдан"
        ></h-text-field>
      </div>
      <div class="validate-form__item">
        <h-text-field
          v-model="ussied_date"
          label="Дата выдачи"
          type="date"
          support_block
          :notification="validate ? $v.ussied_date.$invalid ? 'Обязательное поле' : '' : null"
        ></h-text-field>
      </div>
    </div>

    <div class="validate-form__section">
      <div class="validate-form__item create-button-wrapper">
        <button class="create-button" @click="createClient()">
          Создать
        </button>
      </div>
    </div>
  </div>
</template>

<script>
    import {required, minLength, maxLength, numeric} from 'vuelidate/lib/validators'
    import TextField from '../components/h_text_field.vue'
    import Checkbox from '../components/h_checkbox.vue'
    import Select from './h_select.vue'

  export default {
      components: {
          'h-text-field': TextField,
          'h-select': Select,
          checkbox: Checkbox
      },
      data() {
          return {
              validate: false,

              surname: '',
              name: '',
              second_name: '',
              birthday: '',
              phone: '',
              gender: '',
              client_group: [],
              doctor: '',
              send_sms: false,

              index: '',
              country: '',
              region: '',
              city: '',
              street: '',
              house: '',
              type_document: '',
              series: '',
              number: '',
              ussied: '',
              ussied_date: '',

              types_document: ['Паспорт', 'Свидетельство о рождении', 'Вод. удостоверение'],
              gender_arr: ['М', 'Ж'],
              client_group_arr: ['VIP', 'Проблемные', 'ОМС'],
              doctor_arr: ['Иванов', 'Захаров', 'Чернышева'],
          }
      },
      validations: {
          surname: {
              required
          },
          name: {
              required
          },
          birthday: {
              required
          },
          phone: {
              required,
              minLength: minLength(11),
              maxLength: maxLength(11),
              numeric,
              function(val) {return String(val)[0] == 7}
          },
          client_group: {
              function(val) {return val.length}
          },
          city: {
              required
          },
          type_document: {
              required
          },
          ussied_date: {
              required
          }
      },
      methods: {
          createClient() {
              this.validate = true;
              if (!this.$v.$invalid) {
                  this.$root.$emit('add-item-in-notifications', { //это некруто, но во Vuex не вижу надобности
                      title: 'Уведомление',
                      text: 'Клиент успешно добавлен',
                      type: 'success'
                  })
              }
          },
      }
  }
</script>

<style lang="scss">

  $border-radius: 6px;
  $font-size-input: 16px;
  $font-size-title: 22px;
  $main-color: #3d3d3d;

  .validate-form {
    padding: 16px;
    display: flex;
    flex-direction: column;

    .validate-form__section {
      width: 100%;
      display: flex;
      flex-direction: row;
      flex-wrap: wrap;

      .validate-form__title {
        width: 100%;
        padding: 8px;
        font-size: $font-size-title;
        font-weight: 600;
        color: $main-color;
      }

      .validate-form__item {
        width: 25%;
        padding: 8px;
        font-size: $font-size-input;
      }
      .validate-form__item_checkbox-wrapper {
        padding-top: 25px;
        label {
          color: $main-color;
          font-size: 16px;
        }
        input {
          margin-left: 5px;
          height: 20px;
          width: 20px;
        }
      }

      .clients_group, .gender, .doctor {
        display: flex;
        flex-direction: column;
        justify-content: center;
      }

      .create-button-wrapper {
        width: 100%!important;
        display: flex;
        flex-direction: row;
        justify-content: flex-end;
        button {
          padding: 14px 20px;
          text-transform: uppercase;
          border: none;
          border-radius: 9px;
          outline: none;
          cursor: pointer;
          text-transform: uppercase;
          background: #fff;
          transition: 150ms ease-in-out;
          font-size: 15px;
          color: #1097a1;
        }
        button:hover {
          background: #1097a121;
        }
        button:active {
          background: #1097a152;
        }
      }
    }
  }

  @media (max-width: 1159px) {
    .validate-form {
      .validate-form__section {
        .validate-form__item {
          width: 33.333333%;
        }
      }
    }
  }

  @media (max-width: 768px) {
    .validate-form {
      .validate-form__section {
        .validate-form__item {
          width: 50%;
        }
      }
    }
  }

  @media (max-width: 599px) {
    .validate-form {
      .validate-form__section {
        .validate-form__item {
          width: 100%;
        }
      }
    }
  }
</style>
