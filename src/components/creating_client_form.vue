<template>
  <div class="validate-form">
    <div class="validate-form__section">
      <div class="validate-form__title">Основная информация</div>
      <div class="validate-form__item">
        <h-text-field
          label="Фамилия"
          v-model="surname"
        ></h-text-field>
      </div>
      <div class="validate-form__item">
        <h-text-field
          label="Имя"
          v-model="name"
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
          v-model="birthday"
        ></h-text-field>
      </div>
      <div class="validate-form__item">
        <h-text-field
          label="Телефон"
          v-model="phone"
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
        ></h-select>
      </div>
      <div class="validate-form__item">
        <h-select
          label="Лечащий врач"
          v-model="doctor"
          :items="doctor_arr"
        ></h-select>
      </div>
      <div class="validate-form__item">
        <label>Отправлять СМС</label>
        <input type="checkbox" v-model="send_sms">
      </div>
    </div>

    <div class="validate-form__section">
      <div class="validate-form__title">Адрес</div>
      <div class="validate-form__item">
        <h-text-field
          label="Индекс"
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
      <div class="validate-form__title">Адрес</div>
      <div class="validate-form__item">
        <h-select
          v-model="type_document"
          :items="types_document"
          label="Тип документа"
        ></h-select>
      </div>
      <div class="validate-form__item">
        <h-text-field
          v-model="series"
          label="Серия"
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
  import {required} from 'vuelidate/lib/validators'
  import TextField from '../components/h_text_field.vue'
  import Select from './h_select.vue'

  export default {
      components: {
          'h-text-field': TextField,
          'h-select': Select,
      },
      data() {
          return {
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
          }
      },
      methods: {
          createClient() {
              console.log(this.$data);
          }
      }
  }
</script>

<style lang="scss">

  $not-active-input: #c4c4c4;
  $active-input: #8a8a8a;
  $border-radius: 6px;
  $font-size-input: 16px;
  $font-size-title: 24px;
  $main-color: #3d3d3d;

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
  }
  button:hover {
    background: #f7f7f7;
  }
  button:active {
    background: #d9d9d9;
  }

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
        width: 20%;
        padding: 8px;
        font-size: $font-size-input;
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
