<template>
  <div class="registration-content">
  <div class="registration-container">
    <h2>Реєстрація</h2>
    <form @submit.prevent="onSubmit" class="form">
      <!-- Поля для введення -->
      <FormInput label="Емейл" v-model="formData.email" :error="errors.email" @validate="validateEmail" placeholder="Введіть емейл" />
      <FormInput label="Пароль" type="password" v-model="formData.password" :error="errors.password" @validate="validatePassword" placeholder="Введіть пароль" />
      <FormInput label="Прізвище" v-model="formData.lastName" :error="errors.lastName" @validate="validateName('lastName')" placeholder="Введіть прізвище" />
      <FormInput label="Ім'я" v-model="formData.firstName" :error="errors.firstName" @validate="validateName('firstName')" placeholder="Введіть ім'я" />
      <FormInput label="По батькові" v-model="formData.middleName" :error="errors.middleName" @validate="validateName('middleName')" placeholder="Введіть по батькові" />
      <FormInput label="Номер телефону" v-mask="phoneMask" v-model="formData.phone" :error="errors.phone" @validate="validatePhone" placeholder="(0__) ___-____" />
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
          <option value="працівник">Працівник</option>
          <option value="клієнт">Клієнт</option>
        </select>
        <p v-if="errors.role" class="error">{{ errors.role }}</p>
      </div>

      <!-- Завантаження файлу -->
      <div class="form-group">
        <label>Завантажити файл</label>
        <input type="file" @change="onFileChange" class="input-field" />
      </div>

      <!-- Кнопки реєстрації та входу -->
      <button type="submit" :disabled="!formIsValid" class="submit-button">Зареєструватись</button>
      <button @click="goToLogin" class="login-button">Вже маєте акаунт? Увійти</button>
    </form>
  </div>
    <!-- Таблиця з даними -->
    <Table v-if="tableData.length > 0" :data="tableData" @duplicate="duplicateRow" @delete="deleteRow" />
</div>
</template>

<script>
import FormInput from './FormInput.vue';
import Table from './Table.vue';

export default {
  components: { FormInput, Table },
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
      phoneMask: '(###) ###-####',
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
      this.errors.password = this.formData.password.length >= 8 ? '' : 'Пароль має містити більше 8 символів';
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
      const age = this.calculateAge(this.formData.birthDate);
      this.errors.birthDate = (age >= 16 && age <= 100) ? '' : 'Вік має бути між 16 та 100 роками';
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
    calculateAge(birthDate) {
      const today = new Date();
      const birth = new Date(birthDate);
      let age = today.getFullYear() - birth.getFullYear();
      const monthDiff = today.getMonth() - birth.getMonth();
      if (monthDiff < 0 || (monthDiff === 0 && today.getDate() < birth.getDate())) {
        age--;
      }
      return age;
    },
    onSubmit() {
      if (this.formIsValid) {
        this.tableData.push({ ...this.formData });
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
    duplicateRow(index) {
      this.tableData.push({ ...this.tableData[index] });
    },
    deleteRow(index) {
      this.tableData.splice(index, 1);
    },
    goToLogin() {
    }
  }
}
</script>

<style>
.registration-content {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-content: center;
}

.registration-container {
  max-width: 500px;
  padding: 20px;
  background-color: #fff;
  border-radius: 8px;
  box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.1);
  align-self: center;
}

h2 {
  color: #333;
  margin-bottom: 20px;
}

.form {
  display: flex;
  flex-direction: column;
}

.submit-button, .login-button {
  width: 100%;
  padding: 10px;
  margin-top: 10px;
  font-size: 16px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.submit-button {
  background-color: #4CAF50;
  color: white;
}

.submit-button:disabled {
  background-color: #cccccc;
  cursor: not-allowed;
}

.login-button {
  background-color: transparent;
  color: #4CAF50;
  text-decoration: underline;
}

.login-button:hover {
  color: #388e3c;
}
</style>
