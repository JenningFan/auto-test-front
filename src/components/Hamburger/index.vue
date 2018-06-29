<template>
  <div>
    <svg version="1.1" xmlns="http://www.w3.org/2000/svg" width="32" height="32" @click="toggleClick" class="hamburger" v-show="!isActive" style="" viewBox="0 0 32 32">
      <title>indent-increase</title>
      <path d="M0 2h32v4h-32zM12 8h20v4h-20zM12 14h20v4h-20zM12 20h20v4h-20zM0 26h32v4h-32zM0 22v-12l8 6z"></path>
    </svg>
    <svg version="1.1" xmlns="http://www.w3.org/2000/svg" width="32" height="32" @click="toggleClick" class="hamburger" v-show="isActive" style="" viewBox="0 0 32 32">
      <title>indent-decrease</title>
      <path d="M0 2h32v4h-32zM12 8h20v4h-20zM12 14h20v4h-20zM12 20h20v4h-20zM0 26h32v4h-32zM8 10v12l-8-6z"></path>
    </svg>
  </div>
</template>

<script>
export default {
  name: 'hamburger',
  props: {
    isActive: {
      type: Boolean,
      default: false
    },
    toggleClick: {
      type: Function,
      default: null
    }
  }
}
</script>

<style scoped>
.hamburger {
  display: inline-block;
  cursor: pointer;
  width: 20px;
  height: 20px;
  transition: .38s;
  transform-origin: 50% 50%;
}

.hamburger.is-active {
  transform: rotate(0deg);
}
</style>
