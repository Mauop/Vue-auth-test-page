<template>
  <div id="app">
    <div class="wrapper__head">
      <NavBar @openmodal="authWizard = true" />
      <HeadScreen @openmodal="authWizard = true" />
      <transition name="modal">
        <AuthWizard v-if="authWizard" @close="authWizard = false" />
      </transition>
    </div>
  </div>
</template>

<script>
import NavBar from '@/components/NavBar.vue'
import HeadScreen from '@/components/HeadScreen.vue'
import AuthWizard from '@/components/Auth/Wizard.vue'

export default {
  name: 'App',
  components: {
    NavBar,
    HeadScreen,
    AuthWizard
  },
  data() {
    return {
      authWizard: false
    }
  }
}
</script>

<style lang="scss">
@import '@/assets/scss/main.scss';
:root {
  font-size: 16px;
}

* {
  box-sizing: border-box;
}

html {
  font-size: 16px;
}
body {
  margin: 0;
  padding: 0;
  font-size: 16px;
}

#app {
  font-family: 'Roboto', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #242424;
}

.wrapper__head {
  height: 100vh;
}
</style>
