<template >
    <div class="registration-content">
        <div class="registration-content" >
            <form @submit.prevent="onSubmit" class="form">
                <FormInput label="Емейл" v-model="formData.email" :error="errors.email" @validate="validateEmail" placeholder="Введіть емейл" />
                <FormInput label="Пароль" type="password" v-model="formData.password" :error="errors.password" @validate="validatePassword" placeholder="Введіть пароль" />
                <FormInput label="Номер телефону" type="tel" v-mask="'(###) ###-####'" v-model="formData.phone" :error="errors.phone" @validate="validatePhone"  placeholder="(0__) ___-____"/>
                <FormInput label="Прізвище" v-model="formData.lastName" :error="errors.lastName" @validate="validateName('lastName')" placeholder="Введіть прізвище" />
                <FormInput label="Ім'я" v-model="formData.firstName" :error="errors.firstName" @validate="validateName('firstName')" placeholder="Введіть ім'я" />
                <FormInput label="По батькові" v-model="formData.middleName" :error="errors.middleName" @validate="validateName('middleName')" placeholder="Введіть по батькові" />
                <FormInput label="Дата народження" type="date" v-model="formData.birthDate" :error="errors.birthDate" @validate="validateBirthDate" />
                      <!-- Вибір статі -->
                <div class="form-group">
                  <label>Стать</label>
                  <div class="radio-group">
                    <label><input type="radio" value="Чоловік" v-model="formData.gender" /> Чоловік</label>
                    <label><input type="radio" value="Жінка" v-model="formData.gender" /> Жінка</label>
                  </div>
                  <p v-if="errors.gender" class="error">{{ errors.gender }}</p>
                </div>
            
                <!-- Вибір ролі -->
                <div class="form-group">
                  <label>Роль</label>
                  <select v-model="formData.role" @change="validateRole" class="input-field">
                    <option value="">Оберіть роль</option>
                    <option value="Працівник">Працівник</option>
                    <option value="Клієнт">Клієнт</option>
                  </select>
                  <p v-if="errors.role" class="error">{{ errors.role }}</p>
                </div>
                      <!-- Завантаження файлу -->
                <div class="form-group">
                  <label>Завантажити файл</label>
                  <input type="file" @change="onFileChange" class="input-field" />
                </div>
                <button type="submit" :disabled="!formIsValid" class="submit-button">Зареєструватись</button>
            </form>
        </div>
        <DataTable v-if="tableData.length > 0" :data="tableData" @duplicate="duplicateRow" @delete="deleteRow" />
    </div>
</template>

<script>

import FormInput from './FormInput.vue';
import DataTable from './DataTable.vue';
import {mask} from 'vue-the-mask'


export default {
    directives: {mask},
  components: { FormInput, DataTable },
  data() {
    return {
      formData: {
        email: '',
        password: '',
        lastName: '',
        firstName: '',
        middleName: '',
        phone: '',
        birthDate: '',
        gender: '',
        role: '',
        file: null,
      },
      errors: {
        email: '',
        password: '',
        lastName: '',
        firstName: '',
        middleName: '',
        phone: '',
        birthDate: '',
        gender: '',
        role: '',
      },
      tableData: [],
    };
  },
  computed: {
    formIsValid() {
      return Object.values(this.errors).every(error => error === '') &&
          Object.values(this.formData).every(field => field !== '');
    }
  },
  methods: {
    validateEmail() {
      const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
      this.errors.email = emailPattern.test(this.formData.email) ? '' : 'Невірний формат емейлу';
    },
    validatePassword() {
      this.errors.password = this.formData.password.length >= 6 ? '' : 'Пароль має містити більше 8 символів';
    },
    validateName(field) {
      const namePattern = /^[a-zA-ZА-Яа-яІіЇїЄєҐґ]+$/;
      this.errors[field] = namePattern.test(this.formData[field]) ? '' : 'Має містити тільки букви';
    },
    validatePhone() {
      const phonePattern = /^\(0\d{2}\) \d{3}-\d{4}$/;
      this.errors.phone = phonePattern.test(this.formData.phone) ? '' : 'Формат: (0__) ___-____';
    },
    validateBirthDate() {
      this.errors.birthDate = this.formData.birthDate ? '' : 'Вкажіть ваш вік';
    },
    validateGender() {
      this.errors.gender = this.formData.gender ? '' : 'Оберіть стать';
    },
    validateRole() {
      this.errors.role = this.formData.role ? '' : 'Оберіть роль';
    },
    onFileChange(event) {
      this.formData.file = event.target.files[0];
    },
    
    onSubmit() {
      if (this.formIsValid) {
        this.tableData = [...this.tableData, this.formData]

        this.resetForm();
      }
    },
    resetForm() {
      this.formData = {
        email: '',
        password: '',
        lastName: '',
        firstName: '',
        middleName: '',
        phone: '',
        birthDate: '',
        gender: '',
        role: '',
        file: null,
      };
      this.errors = {};
    },
    duplicateRow(localTableData) {
      this.tableData = [...localTableData]
    },
    deleteRow(localTableData) {
        this.tableData = [...localTableData]
    },
    ///полезная хуйня. После перерыва заебашь отправку событий с компонента table. также измени методы что бы изменение data было не по долбоёбски
  }


  }
</script>

<style></style>