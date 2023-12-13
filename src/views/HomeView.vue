<script lang='ts'>
import LoginForm from '@/components/LoginForm.vue';
import { defineComponent } from 'vue';

export default defineComponent({
  components: {
    LoginForm,
  },
  data() {
    return {
      loginFormVisible: false,
    };
  },
  methods: {
    updateShow() {
      this.loginFormVisible = true;
    },
  },
});
</script>

<template>
  <div class='home'>
    <div class='home__header'>
      <div class='home__header-title'>
        <p>Plan, Write, Execute!</p>
      </div>
      <div class='home__header-buttons'>
        <div class='home__header-register'>
          <a href='/register'>Регистрация</a>
        </div>
        <div class='home__header-login'>
          <button type='button' class='home__button-login' @click='updateShow'>
            Вход
          </button>
          <LoginForm v-model:show=loginFormVisible />
        </div>
      </div>
    </div>
    <div class='home__body'>
      <div class='home__body-text'>
        <p>Время дорого…</p>
        <p>Тратьте его с умом</p>
      </div>
      <div class='home__body-image'>
        <img src='../assets/images/home-page-logo.png' alt='todos'>
      </div>
    </div>
  </div>
</template>

<style scoped>
.home__header {
  display: flex;
  justify-content: space-between;
  margin: 1% 5%;
}

.home__header-buttons {
  width: 20%;
  display: flex;
  justify-content: space-between;
  align-items: center
}

.home__header-title {
  color: rgba(198, 60, 85, 0.81);
  font-family: 'Comfortaa', sans-serif;
  font-size: 64px;
  font-style: normal;
  font-weight: 700;
}

.home__header-title p {
  margin: 0;
}

.home__header-register {
  width: 50%;
}

.home__header-register a {
  color: black;
  font-family: 'Numans', sans-serif;
  font-size: 20px;
  font-style: normal;
  font-weight: 400;
  line-height: normal;
}

.home__header-login {
  width: 35%;
  height: 60%;
  display: inline-flex;
  align-items: center;
}

.home__button-login {
  border-radius: 20px;
  border: #FE746A 2px solid;
  background: #FFFEFE;
  text-align: center;
  height: 100%;
  width: 100%;
  color: #FE746A;
  font-family: 'Numans', sans-serif;
  font-size: 20px;
  font-weight: 400;
  cursor: pointer;
}

.home__button-login:hover {
  background: #FE746A;
  color: #FFFEFE;
}

.home a {
  text-decoration: none;
}

.home__header-login a {
  width: 100%;
  height: 60%;
}

.home__body-text {
  color: rgba(0, 0, 0, 0.80);
  text-shadow: 0 4px 4px rgba(0, 0, 0, 0.30);
  font-family: 'Comfortaa', sans-serif;
  font-size: 48px;
  font-style: normal;
  font-weight: 700;
  line-height: normal;
  width: 52%;
  align-self: normal;
  padding-top: 10%;
}

.home__body {
  margin-left: 5%;
  display: flex;
  align-items: center;
}

.home__body-text p {
  margin: 0;
}

.home__body-image img {
  width: 85%;
}
</style>
