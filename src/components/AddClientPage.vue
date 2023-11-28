<template>
  <div class="add-client__page">
    <div class="add-client__container _container">
      <form  v-on:submit.prevent="submit">
        <h2>Добавление нового клиента</h2>
        <h6>Основная информация</h6>
        <div class="input__container">
          <div class="form-group" :class="{ 'form-group--error': $v.form.lastName.$error }">
            <label>Фамилия*</label>
            <input  type="text" v-model.trim="$v.form.lastName.$model">
            <div class="form__error">
              <div v-if="$v.form.lastName.$error && !$v.form.lastName.required">Поле обязательное для заполнения</div>
            </div>
          </div>
          <div class="form-group" :class="{ 'form-group--error': $v.form.firstName.$error }">
            <label>Имя*</label>
            <input  type="text" v-model.trim="$v.form.firstName.$model">
            <div class="form__error">
              <div v-if="$v.form.firstName.$error && !$v.form.firstName.required">Поле обязательное для заполнения</div>
            </div>
          </div>
          <div class="form-group">
            <label>Отчество</label>
            <input  type="text" v-model.trim="$v.form.fatherName.$model">
            <div class="form__error"></div>
          </div>
          <div class="form-group" :class="{ 'form-group--error': $v.form.dateOfBirth.$error }">
            <label>Дата рождения*</label>
            <input  type="text" v-model.trim="$v.form.dateOfBirth.$model">
            <div class="form__error">
              <div v-if="$v.form.dateOfBirth.$error && !$v.form.dateOfBirth.required">Поле обязательное для заполнения</div>
            </div>
          </div>
          <div class="form-group" :class="{ 'form-group--error': $v.form.phone.$error }">
            <label>Номер телефона*</label>
            <input type="tel" maxlength="11" v-model.trim="$v.form.phone.$model">
            <div class="form__error">
              <div v-if="$v.form.phone.$error && !$v.form.phone.required">Поле обязательное для заполнения</div>
              <div v-if="$v.form.phone.$error && !$v.form.phone.vphone">Номер телефона должен начинаться с 7</div>
              <div v-if="$v.form.phone.$error && (!$v.form.phone.minLength || !$v.form.phone.numeric)">Не корректный номер телефона</div>
            </div>
          </div>
          <div class="form-group">
            <label>Пол</label>
            <multiselect
              v-model="form.gender"
              :options="genders"
              :searchable="false"
              open-direction="bottom"
              placeholder=""
            />
              <div class="form__error"></div>
          </div>
          <div class="form-group" :class="{ 'form-group--error': $v.form.group.$error }">
            <label>Группа клиентов*</label>
            <multiselect
              v-model="form.group"
              :options="groups"
              :multiple="true"
              :searchable="false"
              open-direction="bottom"
              placeholder=""
            />
            <div class="form__error">
              <div v-if="$v.form.group.$error && !$v.form.group.required">Поле обязательное для заполнения</div>
            </div>
          </div>
          <div class="form-group">
            <label>Лечащий врач </label>
            <multiselect 
              v-model="form.doctor"
              :options="doctors"
              :searchable="false"
              open-direction="bottom"
              placeholder=""
            />
            <div class="form__error"></div>
          </div>
          <div class="form-group checkbox">
            <label class="custom-checkbox">
              <input type="checkbox" v-model="form.toggle" true-value="да" false-value="нет">
              <span>Не отправлять СМС</span>
            </label>
            <div class="form__error"></div>
          </div>
        </div>
        <h6>Адресс</h6>
        <div class="input__container">
          <div class="form-group">
            <label>Индекс</label>
            <input type="text" v-model="form.index">
            <div class="form__error"></div>
          </div>
          <div class="form-group">
            <label>Страна</label>
            <input type="text" v-model="form.country">
            <div class="form__error"></div>
          </div>
          <div class="form-group">
            <label>Область</label>
            <input type="text" v-model="form.region">
            <div class="form__error"></div>
          </div>
          <div class="form-group" :class="{ 'form-group--error': $v.form.city.$error }">
            <label>Город*</label>
            <input  type="text" v-model.trim="$v.form.city.$model">
            <div class="form__error">
              <div v-if="$v.form.city.$error && !$v.form.city.required">Поле обязательное для заполнения</div>
            </div>
          </div>
          <div class="form-group">
            <label>Улица</label>
            <input type="text" v-model="form.street">
            <div class="form__error"></div>
          </div>
          <div class="form-group">
            <label>Дом</label>
            <input type="text" v-model="form.house">
            <div class="form__error"></div>
          </div>
        </div>
        <h6>Паспортные данные</h6>
        <div class="input__container">
          <div class="form-group" :class="{ 'form-group--error': $v.form.documentType.$error }">
            <label>Тип документа*</label>
            <multiselect
              v-model="form.documentType"
              :options="documentsTypes"
              :searchable="false"
              open-direction="bottom"
              placeholder=""
            />
            <div class="form__error">
              <div v-if="$v.form.documentType.$error && !$v.form.documentType.required">Поле обязательное для заполнения</div>
            </div>
          </div>
          <div class="form-group">
            <label>Серия</label>
            <input type="text" v-model="form.seriesOfDocuments">
            <div class="form__error"></div>
          </div>
          <div class="form-group">
            <label>Номер</label>
            <input type="text" v-model="form.numberOfDocuments">
            <div class="form__error"></div>
          </div>
          <div class="form-group">
            <label>Кем выдан</label>
            <input type="text" v-model="form.issuedByOfDocuments">
            <div class="form__error"></div>
          </div>
          <div class="form-group" :class="{ 'form-group--error': $v.form.dateOfIssueOfDocuments.$error }">
            <label for="">Дата выдачи*</label>
            <input type="text" v-model="form.dateOfIssueOfDocuments">
            <div class="form__error">
              <div v-if="$v.form.dateOfIssueOfDocuments.$error && !$v.form.dateOfIssueOfDocuments.required">Поле обязательное для заполнения</div>
            </div>
          </div>
        </div>
        <button type="submit">Добавить клиента</button>
      </form>
    </div>
    <notification-form ref="notificationForm"></notification-form>
  </div>
</template>

<script>
import Multiselect from 'vue-multiselect';
import { validationMixin } from 'vuelidate';
import { required, minLength, numeric, helpers } from 'vuelidate/lib/validators';
import NotificationForm from './Notification.vue';

const vphone = helpers.regex('vphone', /^7/);

export default {
  name: 'addClientPage',
  mixins: [validationMixin],
  submitStatus: null,
  components: {
    Multiselect,
    NotificationForm,
  },
  data() {
    return {
      form: {
        lastName: '',
        firstName: '',
        fatherName: '',
        dateOfBirth: '',
        phone: '',
        group: [],
        doctor: '',
        gender: '',
        toggle: '',
        index: '',
        country: '',
        region: '',
        city: '',
        street: '',
        house: '',
        documentType: '',
        seriesOfDocuments: '',
        numberOfDocuments: '',
        issuedByOfDocuments: '',
        dateOfIssueOfDocuments: '',
      },
      genders: ['Мужской', 'Женский'],
      doctors: ['Иванов', 'Захаров', 'Чернышева'],
      groups: ['VIP', 'Проблемные', 'ОМС'],
      documentsTypes: ['Паспорт', 'Свидетельство о рождении', 'Вод. удостоверение'],
    }
  },
  validations: {
    form: {
        lastName: { required },
        firstName: { required },
        fatherName: '',
        dateOfBirth: { required },
        phone: { required, vphone, numeric, minLength: minLength(11) },
        group: { required },
        doctor: '',
        gender: '',
        toggle: '',
        index: '',
        country: '',
        region: '',
        city: { required },
        street: '',
        house: '',
        documentType: { required },
        seriesOfDocuments: '',
        numberOfDocuments: '',
        issuedByOfDocuments: '',
        dateOfIssueOfDocuments: { required },
      },
  },
  methods: {
    showNotification() {
      this.$refs.notificationForm.notificationIsShow = true;
      this.$refs.notificationForm.hideNotification();
    },
    submit() {
      this.$v.$touch()
      if (this.$v.$invalid) {
        this.submitStatus = 'ERROR'
      } else {
        this.showNotification();
      }
    }
  },

}
</script>


