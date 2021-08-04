<template>
  <v-app>
    <v-app-bar app color="blue">
      <v-toolbar-title >Eagle Finance Services</v-toolbar-title>
      <v-spacer></v-spacer>
      <div class="hidden-xs-only">
        <div class="hidden-xs-only">
          <v-btn flat dark text rounded
                 @click="home()">Home
          </v-btn>

          <v-btn
          v-for="item in menu"
          v-if="authenticated"
          :key="item.title"
          :to="item.url"
          flat
          text
          rounded
          dark
        >{{ item.title }}</v-btn>
        <v-btn flat dark text rounded v-if="!authenticated"
          <v-btn flat dark text rounded v-if="!authenticated"
                 @click="register">Register
          </v-btn>

          <v-btn flat dark text rounded v-if="!authenticated"
                 @click="login">login
          </v-btn>

          <v-btn flat dark text rounded v-if="authenticated"
               @click="logout">Log Out
        </v-btn>
      </div>
      <div class="hidden-sm-and-up">
        <v-menu bottom left>
          <template v-slot:activator="{ on, attrs }">
            <v-btn
              dark
              icon
              v-bind="attrs"
              v-on="on"
            >
              <v-icon>mdi-dots-vertical</v-icon>
            </v-btn>
          </template>
          <v-list>
            <v-list-item  v-for="item in menu"
                          :key="item.title"
                          :to="item.url"
            >{{ item.title }}</v-list-item>
            <v-list-item v-if="!authenticated"
                         @click="login">Log in</v-list-item>
            <v-list-item v-if="authenticated"
                         @click="logout">Log out</v-list-item>
          </v-list>
        </v-menu>
      </div>
      </div>
    </v-app-bar>
    <v-content>
      <router-view/>
    </v-content>
  </v-app>

</template>

<script>
import router from './router';
import {APIService} from './http/APIService';
const apiService = new APIService();

export default {
  name: 'App',
  data: () => ({
    authenticated: false,
    dialog: false,
    uName: "Guest",
    menu: [
      { title: 'Home', url:"/"},
      { title: 'Customers', url:"/customer-list" },
      { title: 'Stocks', url:"/stock-list" },
      { title: 'Investments', url:"/investment-list" },
    ]
  }),

  mounted() {
      this.fillMenu();
      apiService.getCustomerList().then(response => {
      this.authenticated = true;
    }).catch(error => {
      if (error.response.status === 401) {
        localStorage.removeItem('isAuthenticates');
        localStorage.removeItem('log_user');
        localStorage.removeItem('token');
        this.authenticated = false;
      }
    });
    console.log('this.authenticated--'+this.authenticated);
  },

  methods: {
    fillMenu(){
      if (localStorage.getItem("isAuthenticates")

        && JSON.parse(localStorage.getItem("isAuthenticates")) ===
        true)
      {
        console.log("IS AUTH")

        this.uName = JSON.parse(localStorage.getItem("log_user"));
      } else {
        console.log("IS NOT AUTH")
      }
      this.menu = [
        { title: 'Movies', url:"/movie-list" }
      ]
    },
    getUser() {
      if (localStorage.getItem("isAuthenticates")

        && JSON.parse(localStorage.getItem("isAuthenticates")) ===
        true) {
        console.log("IS AUTH")
        this.validUserName =
          JSON.parse(localStorage.getItem("log_user"));
      } else {
        console.log("IS NOT AUTH")
      }
    },

    logout() {
      localStorage.removeItem('isAuthenticates');
      localStorage.removeItem('log_user');
      localStorage.removeItem('token');
      this.authenticated = false;
      // router.push('/');
      window.location = "/"
    },
    home() {
      window.location = "/"
    },
    register() {
      router.push('/register');
    },

    login() {
      router.push("/auth");
    },
  }
};

</script>
