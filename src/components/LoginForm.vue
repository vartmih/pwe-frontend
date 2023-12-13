<script lang='ts'>
import { defineComponent } from 'vue';
import axios from 'axios';

export default defineComponent({
  name: 'LoginForm',
  props: {
    show: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      email: '',
      password: '',
      errorStatusCode: 0,
    };
  },
  computed: {
    errorMessage() {
      if (this.errorStatusCode === 400) {
        return 'Неверный логин или пароль';
      }

      return 'Неизвестная ошибка';
    },
  },
  methods: {
    hideLoginForm() {
      this.$emit('update:show', false);
    },
    authorization() {
      axios.post(
        `${process.env.VUE_APP_API_URL}/users/login`,
        { username: this.email, password: this.password },
        {
          headers: {
            'Content-Type': 'application/x-www-form-urlencoded',
          },
          withCredentials: true,
        },
      )
        .then(() => this.hideLoginForm())
        .catch((error) => {
          this.errorStatusCode = error.response.status;
        });
    },
  },
});

</script>

<template>
  <div class='login-form' v-if='show' @click='hideLoginForm' @keydown='hideLoginForm'>
    <div class='login-form__content content-form' @click.stop @keydown.stop>
      <div class='content-form__title'>Авторизуйся</div>
      <div class='content-form__error' v-show='errorStatusCode'>{{ errorMessage }}</div>
      <input
        class='content-form__field'
        type='email'
        aria-label='E-mail'
        placeholder='E-mail'
        v-model='email'
      >
      <input
        class='content-form__field'
        type='password'
        aria-label='password'
        placeholder='Пароль'
        v-model='password'
      >
      <button class='content-form__button' type='submit' @click='authorization'>Войти</button>
      <div class='content-form__register'>Нет аккаунта? <a href='#'>Зарегистрируйся</a></div>
    </div>
  </div>
</template>

<style scoped>
.login-form {
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

.login-form__content {
  background-color: white;
  width: 30%;
  height: 40%;
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

.content-form__field {
  width: 60%;
  height: 10%;
  border-radius: 10px;
  border: 1px solid #CFCDCD;
  background: #FFF;
  padding-left: 3%;
  margin-top: 3%;
}

.content-form__button {
  width: 25%;
  height: 10%;
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

.content-form__register {
  font-family: 'Numans', sans-serif;
  font-size: 15px;
  margin-top: 15%;
}

.content-form__register a {
  color: #FC7B77;
  text-decoration: none;
}

.content-form__register a:hover {
  text-decoration: underline;
}

.content-form__error {
  font-family: 'Numans', sans-serif;
  font-size: 15px;
  color: red;
  margin-top: 2%;
}
</style>
