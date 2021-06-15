<template>
  <transition>
    <slot></slot>
  </transition>
</template>

<script>
export default {
  name: "MTransition",
};
</script>

<style>
.wrapper-enter-from {
  opacity: 0;
}
.wrapper-enter-to {
  opacity: 1;
}
.wrapper-enter-active {
  transition: all 1s ease;
}
</style>
