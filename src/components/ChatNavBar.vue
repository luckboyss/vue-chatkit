<template>
  <b-navbar id="chat-navbar" toggleable="md" type="dark" variant="info">
    <b-navbar-brand href="#">
      Vue 聊天室
    </b-navbar-brand>
    <b-navbar-nav class="ml-auto">
      <b-nav-text>{{ user.name }} | </b-nav-text>
      <b-nav-item href="#" @click="onLogout" active>退出</b-nav-item>
    </b-navbar-nav>
  </b-navbar>
</template>

<script>
import { mapState, mapActions, mapMutations } from 'vuex'

export default {
  name: 'ChatNavBar',
  computed: {
    ...mapState(['user', 'reconnect', 'activeRoom'])
  },
  methods: {
    ...mapActions(['logout', 'login']),
    ...mapMutations(['setReconnect']),
    onLogout() {
      this.$router.push({ path: '/' })
      this.logout()
    },
    unload() {
      if (this.user.username) {
        this.setReconnect(true)
      }
    }
  },
  mounted() {
    window.addEventListener('beforeunload', this.unload)
    if (this.reconnect) {
      this.login({
        userId: this.user.username,
        currentRoom: this.activeRoom
      })
    }
  }
}
</script>

<style>
#chat-navbar {
  margin-bottom: 15px;
}
</style>
