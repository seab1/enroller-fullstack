<template>
  <div id="app">
    <h1>
      <img src="./assets/logo.svg" alt="Enroller" class="logo">
      System do zapisów na zajęcia
    </h1>
    <div v-if="authenticatedUsername">
      <h2>Witaj {{ authenticatedUsername }}!
        <a @click="logout()" class="float-right  button-outline button">Wyloguj</a>
      </h2>
      <meetings-page :username="authenticatedUsername"></meetings-page>
    </div>
    <div v-else>
      <button @click="changeToLogin()" :class="currentState == 'toLogout' ? 'button-outline' : ''">Go to login form</button>
      <button @click="changeToLogout()" :class="currentState == 'toLogin' ? 'button-outline' : ''">Go to logout form</button>
      <login-form v-if="currentState == 'toLogin'" @login="login($event)"></login-form>
      <login-form v-else @login="register($event)" button-label="Zarejestruj sie"></login-form>
    </div>
  </div>
</template>

<script>
    import "milligram";
    import LoginForm from "./LoginForm";
    import MeetingsPage from "./meetings/MeetingsPage";

    export default {
        components: {LoginForm, MeetingsPage},
        data() {
            return {
                authenticatedUsername: "",
                currentState: 'toLogin'
            };
        },
        methods: {
            login(user) {
                this.authenticatedUsername = user.login;
            },

            logout() {
                this.authenticatedUsername = '';
            },

            register(user)
            {

            },

            changeToLogin() {this.currentState = 'toLogin';},
            changeToLogout() {this.currentState = 'toLogout';}
        }
    };
</script>

<style>
  #app {
    max-width: 1000px;
    margin: 0 auto;
  }

  .logo {
    vertical-align: middle;
  }
</style>

