<template>
  <v-container fluid>
    <v-slide-y-transition mode="out-in">
      <v-layout column align-center>
        <v-form
        v-if="!loading"
        v-model="valid"
        @submit.prevent="login"
        @keydown.prevent.enter>
            <v-text-field
              v-model="user.username"
              :rules="notEmptyRules"
              label="Username"
              required
            ></v-text-field>
            <v-text-field
              v-model="user.password"
              :rules="notEmptyRules"
              label="Password"
              type= "password"
              required
            ></v-text-field>
            <v-btn type="submit" :disabled="!valid">Login</v-btn>
         </v-form>
         <v-progress-circular
          v-if="loading"
          :size="70"
          :width="7"
          indeterminate
          color="primary">
        </v-progress-circular>
      </v-layout>
    </v-slide-y-transition>
  </v-container>
</template>

<script>
import { mapState, mapActions } from 'vuex';

export default {
  name: 'login',
  data: () => ({
    valid: false,
    user: {
      username: '',
      password: '',
    },
    notEmptyRules: [v => !!v || 'E-mail is required'],
  }),
  computed: {
    ...mapState('auth', { loading: 'isAuthenticatePending' }),
  },
  methods: {
    ...mapActions('auth', ['authenticate', 'logout']),
    async login() {
      if (this.valid) {
        this.authenticate({
          strategy: 'local',
          username: this.user.username,
          password: this.user.password,
        }).then(async () => {
          this.$router.push('/boards');
        }).catch((e) => {
          // eslint-disable-next-line
          console.error('Authentication error', e);
        });
      }
    },
  },
};
</script>
