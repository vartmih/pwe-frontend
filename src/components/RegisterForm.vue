<script lang='ts'>
import { defineComponent } from 'vue';
import axios from 'axios';
import { Form, Field } from 'vee-validate';
import { object, string } from 'yup';
import NotificationPopup from '@/components/NotificationPopup.vue';

export default defineComponent({
  name: 'RegisterForm',
  components: {
    Form,
    Field,
    NotificationPopup,
  },
  props: {
    show: {
      type: Boolean,
      default: false,
    },
  },
  setup() {
    const registerSchema = object({
      email: string().email('Неверный E-mail').required('Обязательное поле'),
      first_name: string().required('Обязательное поле'),
      last_name: string().required('Обязательное поле'),
      birth_date: string().required('Обязательное поле'),
      password: string().required('Обязательное поле').min(8, 'Минимальная длина пароля 8 символов'),
      repeatPassword: string().required('Обязательное поле').min(8, 'Минимальная длина пароля 8 символов'),
    });
    return {
      registerSchema,
    };
  },
  data() {
    return {
      errorStatusCode: 0,
      showDatePicker: false,
      showNotification: false,
      successMessage: 'Вы успешно зарегистрированы!',
    };
  },
  computed: {
    errorMessage() {
      if (this.errorStatusCode === 400) {
        return 'Неверные данные';
      }

      return 'Неизвестная ошибка';
    },
  },
  methods: {
    hideRegisterForm() {
      this.$emit('update:show', false);
    },

    updateShowNotification() {
      this.showNotification = !this.showNotification;
    },

    register(values: any) {
      console.log(values);
      axios.post(
        `${process.env.VUE_APP_API_URL}/users/register`,
        values,
      )
        .then(() => {
          this.updateShowNotification();
          this.hideRegisterForm();
        })
        .catch((error) => {
          this.errorStatusCode = error.response.code;
        });
    },
  },
});
</script>

<template>
  <NotificationPopup v-model:show='showNotification' :text='successMessage'/>
  <Form
    class='register-form'
    v-if='show'
    @click='hideRegisterForm'
    @keydown='hideRegisterForm'
    @submit='register'
    :validation-schema="registerSchema"
    v-slot="{ errors }"
  >
      <div class='register-form__content content-form' @click.stop @keydown.stop>
        <div class='content-form__title'>Зарегистрируйся</div>
        <div class='content-form__error' v-show='errorStatusCode'>{{ errorMessage }}</div>
        <div class='content-form__container'>
          <Field
            class='content-form__field'
            type='email'
            name='email'
            placeholder='E-mail'
            :class='{ "invalid-field": errors.email }'
            :validateOnInput="true"
          />
          <div
            class='content-form__invalid-field'
            v-show='errors.email'
          >
            {{ errors.email }}
          </div>
        </div>
        <div class='content-form__container'>
          <Field
            class='content-form__field'
            type='text'
            name='first_name'
            placeholder='Имя'
            :class='{ "invalid-field": errors.first_name }'
            :validateOnInput="true"
          />
          <div
            class='content-form__invalid-field'
            v-show='errors.first_name'
          >
            {{ errors.first_name }}
          </div>
        </div>
        <div class='content-form__container'>
          <Field
            class='content-form__field'
            type='text'
            name='last_name'
            placeholder='Фамилия'
            :class='{ "invalid-field": errors.last_name }'
            :validateOnInput="true"
          />
           <div
            class='content-form__invalid-field'
            v-show='errors.last_name'
          >
            {{ errors.last_name }}
          </div>
        </div>
        <div class='content-form__container'>
          <Field
            class='content-form__field'
            type='date'
            name='birth_date'
            placeholder='Дата рождения'
            min='1940-01-01'
            max='2024-01-01'
            :class='{ "invalid-field": errors.birth_date }'
            :validateOnInput="true"
          />
         <div
            class='content-form__invalid-field'
            v-show='errors.birth_date'
          >
            {{ errors.birth_date }}
          </div>
        </div>
        <div class='content-form__container'>
          <Field
            class='content-form__field'
            type='password'
            name='password'
            placeholder='Пароль'
            :class='{ "invalid-field": errors.password }'
            :validateOnInput="true"
          />
          <div
            class='content-form__invalid-field'
            v-show='errors.password'
          >
            {{ errors.password }}
          </div>
        </div>
        <div class='content-form__container'>
          <Field
            class='content-form__field'
            type='password'
            name='repeatPassword'
            placeholder='Повторите пароль'
            :class='{ "invalid-field": errors.repeatPassword }'
            :validateOnInput="true"
          />
          <div
            class='content-form__invalid-field'
            v-show='errors.repeatPassword'
          >
            {{ errors.repeatPassword }}
          </div>
        </div>
        <button
          class='content-form__button'
          type='submit'
        >
          Зарегистрироваться
        </button>
        <div class='content-form__login'>Есть аккаунт? <a href='#'>Войти</a></div>
      </div>
    </Form>
</template>

<style scoped>
.register-form {
  display: flex;
  justify-content: center;
  align-items: center;
  position: fixed;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  background-color: rgba(0, 0, 0, 0.5);
}

.register-form__content {
  background-color: white;
  width: 30%;
  height: 60%;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  border-radius: 20px;
}

.content-form__title {
  font-family: 'Numans', sans-serif;
  font-size: 20px;
  color: #FC7B77;
}

.content-form__container {
    width: 60%;
    height: 10%;
}

.content-form__field {
  width: 100%;
  height: 60%;
  border-radius: 10px;
  border: 1px solid #CFCDCD;
  background: #FFF;
  padding-left: 3%;
  padding-right: 3%;
  margin-top: 3%;
}

.content-form__button {
  width: 50%;
  height: 7%;
  border-radius: 10px;
  border: 0;
  background: #FC7B77;
  font-family: 'Numans', sans-serif;
  font-size: 20px;
  margin-top: 5%;
  cursor: pointer;
  color: #ffffff;
}

.content-form__button:hover {
  background: #ffffff;
  color: #FC7B77;
  border: #FC7B77 2px solid;
}

.content-form__login {
  font-family: 'Numans', sans-serif;
  font-size: 15px;
  margin-top: 15%;
}

.content-form__login a {
  color: #FC7B77;
  text-decoration: none;
}

.content-form__login a:hover {
  text-decoration: underline;
}

.content-form__error {
  font-family: 'Numans', sans-serif;
  font-size: 15px;
  color: red;
  margin-top: 2%;
}

input[type="date"] {
    position: relative;
}

input[type="date"]::-webkit-calendar-picker-indicator {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  width: auto;
  height: auto;
  color: transparent;
  background: transparent;
}

input[type="date"]::-webkit-inner-spin-button,
input[type="date"]::-webkit-clear-button {
     z-index: 1;
}

input[type=date] {
  text-align: right;
}

input[type="date"]:before {
  content: attr(placeholder) !important;
  margin-right: 0.5em;
  color: #7e7e7e;
}

.invalid-field {
  border: 1px solid red;
}

.content-form__invalid-field {
  color: red;
  font-size: 15px;
  text-align: left;
}
</style>
