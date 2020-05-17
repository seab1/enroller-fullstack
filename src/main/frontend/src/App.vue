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
      <button @click="changeToLogout()" :class="currentState == 'toLogin' ? 'button-outline' : ''">Go to register form</button>
      <div v-if="userAdditionStatus" class="error" :style="userAdditionStatus != 'User creation error!' ? 'background: #00FF00; border: 3px dotted green;' : ''">{{ userAdditionStatus }}</div>
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
                currentState: 'toLogin',
                userAdditionStatus: ''
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
              this.$http.post('participants', user)
              .then(response => {
                this.userAdditionStatus = 'User created successfully!';
              })
              .catch(response => {
                this.userAdditionStatus = 'User creation error!';
              });
            },

            changeToLogin() {this.currentState = 'toLogin'; this.userAdditionStatus = '';},
            changeToLogout() {this.currentState = 'toLogout'; this.userAdditionStatus = '';}
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

  .error {
    border: 3px dotted red;
    padding: 10px;
    background: pink;
    text-align: center;
  }
</style>

