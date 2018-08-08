<template>
  <div>
    <b-alert show variant="warning">This is a secure page!</b-alert>
    <b-row>
      <b-col md="8">
        <b-card title="State">
          <pre>{{ state }}</pre>
        </b-card>
      </b-col>
      <b-col md="4">
        <b-card title="Scopes" class="mb-2">
          User:
          <b-badge>{{ $auth.hasScope('user') }}</b-badge>
          Test:
          <b-badge>{{ $auth.hasScope('test') }}</b-badge>
          Admin:
          <b-badge>{{ $auth.hasScope('admin') }}</b-badge>
        </b-card>
        <b-card title="token">
          {{ token || '-' }}
        </b-card>
      </b-col>
    </b-row>
    <hr>
    <b-btn-group>
      <b-button @click="$auth.fetchUser()">Fetch User</b-button>
      <b-button @click="$auth.logout()">Logout</b-button>
    </b-btn-group>
  </div>
</template>

<script>
  export default {
    middleware: ['auth'],
    data() {
      return {
        username: 0
      }
    },
    computed: {
      token() {
        console.log(this.$auth.getToken('keycloak'))
        return this.$auth.getToken('keycloak')
      },
      state() {
        return JSON.stringify(this.$auth.$state, undefined, 2)
      }
    },
    async asyncData({
      app,
      query,
      req
    }) {
      // 服务器端获取数据实例
      console.log(app.$axios.defaults.headers.common)
      let data = await app.$axios.$get('/api/user/info')
      return {
        username: 1
      }
    },
  }

</script>
