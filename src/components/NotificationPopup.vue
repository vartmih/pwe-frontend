<script lang='ts'>
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'NotificationPopup',
  props: {
    text: {
      type: String,
      default: '',
    },
    timeout: {
      type: Number,
      default: 5000,
    },
    show: {
      type: Boolean,
      default: false,
    },
  },
  methods: {
    hideNotification() {
      this.$emit('update:show', false);
    },
  },
  updated() {
    setTimeout(() => {
      this.hideNotification();
    }, this.timeout);
  },
});
</script>

<template>
  <div class='notification' v-show='show'>
    <div class='notification__content'>
      <span class='notification__text'>
        {{ text }}
      </span>
      <button class="notification__close" @click='hideNotification'>&#10005;</button>
    </div>
  </div>
</template>

<style scoped>
.notification {
  position: fixed;
  top: 3%;
  right: 1%;
  width: 20%;
  height: 5%;
  background-color: #98f5e1;
}

.notification__content {
  display: flex;
  align-items: center;
  justify-content: space-between;
  color: #236c5b;
  height: 100%;
  padding-left: 4%;
  padding-right: 4%;
  font-family: 'Numans', sans-serif;
  font-size: 15px;
}

.notification__close {
  cursor: pointer;
  border: none;
  background: transparent;
}
</style>
