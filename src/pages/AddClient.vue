<template>
  <section class="add-client__page">
    <section class="add-client__container _container">
      <form @submit.prevent="submit">
        <h2>Добавление нового клиента</h2>
        <h6>Основная информация</h6>
        <section class="input__container">

          <section class="form-group" :class="{ 'form-group--error': $v.form.lastName.$error }">
            <label>Фамилия*</label>
            <input  type="text" v-model.trim="$v.form.lastName.$model">
            <div class="form__error">
              <div v-if="$v.form.lastName.$error && !$v.form.lastName.required">Поле обязательное для заполнения</div>
            </div>
          </section>

          <section class="form-group" :class="{ 'form-group--error': $v.form.firstName.$error }">
            <label>Имя*</label>
            <input  type="text" v-model.trim="$v.form.firstName.$model">
            <div class="form__error">
              <div v-if="$v.form.firstName.$error && !$v.form.firstName.required">Поле обязательное для заполнения</div>
            </div>
          </section>

          <section class="form-group">
            <label>Отчество</label>
            <input  type="text" v-model.trim="$v.form.fatherName.$model">
          </section>

          <section class="form-group" :class="{ 'form-group--error': $v.form.dateOfBirth.$error }">
            <label>Дата рождения*</label>
            <input  type="text" v-model.trim="$v.form.dateOfBirth.$model" @input="replacer('dateOfBirth')">
            <div class="form__error">
              <div v-if="$v.form.dateOfBirth.$error && !$v.form.dateOfBirth.required">Поле обязательное для заполнения</div>
            </div>
          </section>

          <CustomSelector
            class="form-group" 
            :label="'Пол'" 
            :id="'gender'" 
            :options="genders" 
            :value="form.gender" 
            @setvalue="handleSelect"
          />

          <section class="form-group" :class="{ 'form-group--error': $v.form.phone.$error }">
            <label>Номер телефона*</label>
            <input type="tel" maxlength="11" v-model.trim="$v.form.phone.$model" @input="replacer('phone')">
            <div class="form__error">
              <div v-if="$v.form.phone.$error && !$v.form.phone.required">Поле обязательное для заполнения</div>
              <div v-if="$v.form.phone.$error && !$v.form.phone.vphone">Номер телефона должен начинаться с 7</div>
              <div v-if="$v.form.phone.$error && (!$v.form.phone.minLength || !$v.form.phone.numeric)">Не корректный номер телефона</div>
            </div>
          </section>

          <CustomSelector
            class="form-group"
            multiple
            :class="{ 'form-group--error': $v.form.group.$error }"
            :label="'Группа клиентов*'"
            :id="'groups'"
            :options="groups"
            :values="form.group"
            :errors="[
              { 'id': 1, 'show': $v.form.group.$error && !$v.form.group.required, 'message': 'Поле обязательное для заполнения' }
            ]"
            @setvalue="handleSelect"
            @remove="removeGroup"
          />

          <CustomSelector
            class="form-group"
            :label="'Лечащий врач'" 
            :id="'doctor'" 
            :options="doctors" 
            :value="form.doctor"
            @setvalue="handleSelect"
          />

          <section class="form-group checkbox">
            <label class="custom-checkbox">
              <input type="checkbox" v-model="form.toggle">
              <span>Не отправлять СМС</span>
            </label>
            <div class="form__error"></div>
          </section>

        </section>
        <h6>Адрес</h6>
        <section class="input__container">
          <section class="form-group">
            <label>Индекс</label>
            <input type="text" minlength="6" maxlength="6" v-model="form.index" @input="replacer('index')">
            <div class="form__error"></div>
          </section>

          <section class="form-group">
            <label>Страна</label>
            <input type="text" v-model="form.country">
            <div class="form__error"></div>
          </section>

          <section class="form-group">
            <label>Область</label>
            <input type="text" v-model="form.region">
            <div class="form__error"></div>
          </section>

          <section class="form-group" :class="{ 'form-group--error': $v.form.city.$error }">
            <label>Город*</label>
            <input  type="text" v-model.trim="$v.form.city.$model">
            <div class="form__error">
              <div v-if="$v.form.city.$error && !$v.form.city.required">Поле обязательное для заполнения</div>
            </div>
          </section>

          <section class="form-group">
            <label>Улица</label>
            <input type="text" v-model="form.street">
            <div class="form__error"></div>
          </section>

          <section class="form-group">
            <label>Дом</label>
            <input type="text" v-model="form.house">
            <div class="form__error"></div>
          </section>

        </section>
        <h6>Паспортные данные</h6>
        <section class="input__container">

          <CustomSelector
            class="form-group"
            :class="{ 'form-group--error': $v.form.documentType.$error }"
            :label="'Тип документа*'"
            :id="'documentType'"
            :options="documentsTypes"
            :value="form.documentType"
            :errors="[
              { 'id': 1, 'show': $v.form.documentType.$error && !$v.form.documentType.required, 'message': 'Поле обязательное для заполнения' }
            ]"
            @setvalue="handleSelect"
          />

          <section class="form-group">
            <label>Серия</label>
            <input type="text" v-model="form.seriesOfDocuments" @input="replacer('seriesOfDocuments')">
            <div class="form__error"></div>
          </section>

          <section class="form-group">
            <label>Номер</label>
            <input type="text" v-model="form.numberOfDocuments" @input="replacer('numberOfDocuments')">
            <div class="form__error"></div>
          </section>

          <section class="form-group">
            <label>Кем выдан</label>
            <input type="text" v-model="form.issuedByOfDocuments">
            <div class="form__error"></div>
          </section>

          <section class="form-group" :class="{ 'form-group--error': $v.form.dateOfIssueOfDocuments.$error }">
            <label>Дата выдачи*</label>
            <input type="text" maxlength="10" v-model="form.dateOfIssueOfDocuments" @input="replacer('dateOfIssueOfDocuments')">
            <div class="form__error">
              <div v-if="$v.form.dateOfIssueOfDocuments.$error && !$v.form.dateOfIssueOfDocuments.required">Поле обязательное для заполнения</div>
            </div>
          </section>
        </section>
        <button class="button_submit" type="submit">Добавить клиента</button>
      </form>
    </section>
    <footer>
      <div class="footer_container">
      </div>
   </footer>
    <NotificationForm ref="notificationForm"></NotificationForm>
  </section>
</template>

<script>
import { validationMixin } from 'vuelidate';
import { required, minLength, numeric, helpers } from 'vuelidate/lib/validators';
import NotificationForm from '../components/Notification.vue';
import CustomSelector from '../components/CustomSelector.vue';

const vphone = helpers.regex('vphone', /^7/);

export default {
  name: 'addClientPage',
  mixins: [validationMixin],
  submitStatus: null,
  components: { 
    NotificationForm,
    CustomSelector,
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
    removeGroup(value) {
      const index = this.form.group.indexOf(value);
      this.form.group.splice(index, 1);
    },
    handleSelect(key, value) {
      if (key === 'groups') {
        this.form.group.includes(value) ? this.removeGroup(value) : this.form.group.push(value);
      } else {
        this.form[key] = value;
      }
    },
    replacer(key) {
      this.form[key] = this.form[key].replace(/[^0-9+]/g, '');
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


