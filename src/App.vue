<template>
  <router-view v-slot="{ Component }">
    <transition name="wrapper" appear>
      <div class="bg-gray-900 w-full h-screen flex flex-col">
        <component :is="Component" />
      </div>
    </transition>
  </router-view>
</template>

<script>
import MTransition from "./components/MTransition";
export default {
  name: "App",
  components: { MTransition },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

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
