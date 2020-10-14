<template>
  <app-navigator>

  </app-navigator>
</template>


<script>

var firebase = require("firebase/app");
require("firebase/database");

const firebaseConfig = {
  apiKey: "AIzaSyBvmq6oHdrKRBUW5r98qVJdMGkXnd7JT40",
  authDomain: "projectju-7504d.firebaseapp.com",
  databaseURL: "https://projectju-7504d.firebaseio.com",
  projectId: "projectju-7504d",
  storageBucket: "projectju-7504d.appspot.com",
  messagingSenderId: "870066259121",
  appId: "1:870066259121:web:0a11fd1b568439e40bce90",
  measurementId: "G-DLJQQE49E1"
};


import {
  createAppContainer,
  createStackNavigator,
} from "vue-native-router";

import LoginScreen from "./components/login";
import signUpScreen from "./components/signUp"
import controllerScreen from "./components/controller"


const StackNavigator = createStackNavigator(
  {
    Login: LoginScreen,
    Signup: signUpScreen,
    Control : controllerScreen
  },
  {
    initialRouteName: 'Login',
  }
);

const AppNavigator = createAppContainer(StackNavigator);


export default {
  
  components: { 
    AppNavigator,
  },

  async mounted() {

    firebase.initializeApp(firebaseConfig);
  }
}
</script>


<style>
.container {
  background-color: white;
  align-items: center;
  justify-content: center;
  flex: 1;
}
.text-color-primary {
  color: blue;
}
</style>
