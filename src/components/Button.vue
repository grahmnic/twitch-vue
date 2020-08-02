<template>
  <button v-on:click="callback" :style="style">
    <slot />
  </button>
</template>

<script>
export default {
  name: 'Button',
  props: {
    callback: Function,
    width: String,
    height: String
  },
  computed: {
    style() {
      return 'width: ' + this.width + 
             ';height: ' + this.height + ';';
    }
  }
}
</script>

<style lang="scss" scoped>
button {
  border: none;
  font-size: 1.5rem;
  border-radius: 0.3rem;
  padding: 4px 6px;
  color: $light;
  background-color: $primary;
  width: 4rem;
  height: 1.5rem;
  cursor: pointer;
  border: 1px solid $primary;
  transition: all 0.25s ease-in-out;
  transform: translateY(-2px);
  box-shadow: 0px 2px $primary-dark, 0px 3px $primary-dark;
  &:hover {
    transform: translateY(-5px);
    box-shadow: 0px 5px $primary-dark, 0px 6px $primary-dark;
  }
  &:active {
    transform: translateY(1px);
    background-color: $primary-dark;
    color: $off-white;
    box-shadow: none;
  }
  &:focus {
    outline: none;
  }
}
</style>
