<template>
  <q-layout view="lHh Lpr lFf">
    <q-layout-header v-model="show" reveal>
      <q-toolbar color="white">
        <q-toolbar-title align="left">
          <div class="row ">
            <a href="https://www.capgemini.com/no-no/" target="_blank">
              <img src="../assets/capgemini_logo.svg" class="" >
            </a>
          </div>
        </q-toolbar-title>
        <q-btn :to="'/howto'" flat class="text-black">How to play</q-btn>
        <q-btn v-if="loggedInUser === false" @click.native="openLogin('top')"
        flat class="text-black">Sign in</q-btn>
        <q-btn v-if="loggedInUser === true && this.$route.path !== '/profile'" :to="'/profile'" flat
        class="text-black">Profile</q-btn>
      </q-toolbar>
      <router-link to="/">
        <div class="arl_toolbar" v-if="loggedInUser === false || this.$route.path !== '/'">
          <div class="row justify-between">
            <h2 class="text-white col-4 q-pa-md">AI Rocket league</h2>
            <div class="toolbar_overlay col-2 q-pa-md gt-md" v-if="loggedInUser === false">
              <h5 class="text-secondary">
                Sign up today!
              </h5>
              <p class="text-secondary">You are just one step away from signing up
                and joining the league!
              </p>
              <q-btn class="bg-white" @click.native="openSignUp">Sign up</q-btn>
            </div>
          </div>
        </div>
      </router-link>
    </q-layout-header>

    <q-page-container>
      <q-modal v-model="loginModal" :position="position" width="30%">
        <div class="loginModal row justify-center">
          <div class="q-display-1 q-mb-md col-12">LOGIN</div>
          <div class="inputWrapper">
            <q-input
            v-model="form.username"
            float-label="Username"
            @keyup.enter="submit"
            />
            <q-input
            v-model="form.password"
            float-label="Password"
            type="password"
            @keyup.enter="submit"
            />
            <div class="row q-pb-md">
              <q-btn flat class="q-ml-sm q-mb-sm q-mt-sm col-3"
              @click.native="openSignUp()">Sign up</q-btn>
              <q-btn color="primary" class="q-ma-sm offset-3" @click="login">Login</q-btn>
              <q-btn color="orange" class="q-ml-sm q-mb-sm q-mt-sm"
              @click="loginModal = false" label="Close" />
            </div>
          </div>
        </div>
      </q-modal>
      <q-modal v-model="signUpModal" width="30%">
        <div class="loginModal row justify-center">
          <div class="q-display-1 q-mb-md col-12">SIGN UP</div>
          <div class="inputWrapper">
            <q-input
            v-model="form.username"
            float-label="Username"
            @keyup.enter="submit"
            />
            <q-input
            v-model="form.password"
            float-label="Password"
            type="password"
            @keyup.enter="submit"
            />
            <div class="row float-right q-pb-md">
              <q-btn color="primary" class="q-ma-sm" @click="login">Sign up</q-btn>
              <q-btn color="orange" class="q-ml-sm q-mb-sm q-mt-sm"
              @click="signUpModal = false" label="Close" />
            </div>
          </div>
        </div>
      </q-modal>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import { openURL } from 'quasar';

export default {
  name: 'MyLayout',
  data() {
    return {
      show: true,
      loginModal: false,
      signUpModal: false,
      position: 'top',
      form: {
        username: '',
        password: '',
      },
      loggedInUser: false,
    };
  },
  methods: {
    openURL,
    openLogin(position) {
      this.position = position;
      this.$nextTick(() => {
        this.loginModal = true;
      });
    },
    login() {
      const userData = {
        username: this.form.username,
      };
      this.$store.commit('user/default', userData);
      this.loginModal = !this.loginModal;
      this.loggedInUser = !this.loggedInUser;
      this.$router.push({ path: '/profile' });
    },
    openSignUp() {
      this.$nextTick(() => {
        this.loginModal = false;
        this.signUpModal = true;
      });
    },
  },

};
</script>

<style>
</style>
