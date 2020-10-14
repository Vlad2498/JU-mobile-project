<template>
  <app-navigator>

  </app-navigator>
</template>


<script>

var firebase = require("firebase/app");
require("firebase/database");

const firebaseConfigDevice = {
    // apiKey: "AIzaSyBvmq6oHdrKRBUW5r98qVJdMGkXnd7JT40",
    // authDomain: "projectju-7504d.firebaseapp.com",
    // databaseURL: "https://projectju-7504d.firebaseio.com",
    // projectId: "projectju-7504d",
    // storageBucket: "projectju-7504d.appspot.com",
    // messagingSenderId: "870066259121",
    // appId: "1:870066259121:web:91abc5f9661776cc0bce90",
    // measurementId: "G-VYWHJR0VJZ"
    
      apiKey: "AIzaSyBvmq6oHdrKRBUW5r98qVJdMGkXnd7JT40",
      authDomain: "projectju-7504d.firebaseapp.com",
      databaseURL: "https://projectju-7504d.firebaseio.com",
      projectId: "projectju-7504d",
      storageBucket: "projectju-7504d.appspot.com",
      messagingSenderId: "870066259121",
      appId: "1:870066259121:web:0a11fd1b568439e40bce90",
      measurementId: "G-DLJQQE49E1"
};

// var device = firebase.initializeApp(firebaseConfigDevice);

import {
  createAppContainer,
  createStackNavigator,
} from "vue-native-router";



import windowScreen from "./components/windows"
const StackNavigator = createStackNavigator(
  {
    Window: windowScreen,
  },
  {
    initialRouteName: 'Window',
  }
);

const AppNavigator = createAppContainer(StackNavigator);

export default {
  
  components: { 
    AppNavigator,
  },

  async mounted() {

    firebase.initializeApp(firebaseConfigDevice);
  }
}

</script>


<style>

</style>
