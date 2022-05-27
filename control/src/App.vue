<template>
  <transition name="fade">
    <openerComp v-if="showOpener"/>
  </transition>
  <transition name="fade">
    <div v-if="!showOpener">
      <router-view/>
      <AppHeader/>
      <AppFooter/>
    </div>
  </transition>
</template>

<script>
import { onMounted, ref } from "vue"
import AppHeader from "./components/AppHeader.vue"
import AppFooter from "./components/AppFooter.vue"
import openerComp from "./components/opener.vue"

export default {
  name: 'App',
  components: {
    AppHeader,
    AppFooter,
    openerComp
  },
  setup() {
    const showOpener = ref(true)

    function showOpenerFunc() {
      showOpener.value = false
    }

    onMounted(() => {
      setTimeout(showOpenerFunc, 2000)
    })

    return { showOpener, showOpenerFunc }
  }
}
</script>

<style>
    * {
      user-select: none;
    }
    .fade-enter-active,
    .fade-leave-active {
    transition: opacity 0.5s ease;
    }

    .fade-enter-from {
    opacity: 0;
    }

    .fade-enter-to {
    opacity: 1;
    }

    .fade-leave-from {
    opacity: 1;
    }

    .fade-leave-to {
    opacity: 0;
    }
</style>
