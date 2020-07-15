<template>
  <div class="main">
    <!-- <div class="main-links">
      <nuxt-link to="/">Home Page</nuxt-link>|
      <div @click="logout" v-if="loggedIn" class="logout-link">Logout</div>
      <nuxt-link v-else to="/login">Login Page</nuxt-link>|
      <nuxt-link to="/secret">Secret Page</nuxt-link>
    </div> -->
    <nuxt />
  </div>
</template>
<script>
import Cookies from 'js-cookie'
import * as firebase from 'firebase/app'
import 'firebase/auth'

export default {
  data() {
    return {
      loggedIn: false
    }
  },
  mounted() {
    this.setupFirebase()
  },
  asyncData() {},
  methods: {
    setupFirebase() {
      firebase.auth().onAuthStateChanged(user => {
        if (user) {
          // User is signed in.
          console.log('signed in')
          firebase
            .auth()
            .currentUser.getIdToken(true)
            .then(token => Cookies.set('access_token', token))

          this.loggedIn = true
        } else {
          Cookies.remove('access_token')

          // if (Cookies.set('access_token', 'blah')) {
          // }

          // No user is signed in.
          this.loggedIn = false
          console.log('signed out', this.loggedIn)
        }
      })
    },
    logout() {
      firebase
        .auth()
        .signOut()
        .then(() => {
          this.$router.replace({ name: 'login' })
        })
    }
  }
}
</script>

