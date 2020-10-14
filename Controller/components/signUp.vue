<template>
  <view class="container">
    <!-- <header title="Sign up" />     -->

  <view>
    <text class="AppTitle">Smart Window</text>

    <!-- E-mail field -->
    <text-input 
    class = "text-input-container"
    placeholder = "E-mail"
    v-model="email"
    />
    
    <!-- Password field-->
    <text-input 
    class = "text-input-container"
    placeholder = "Password"
    v-model="password"
    secureTextEntry
    />
  </view>

  <!-- Error messages -->
  <view v-if="(error!='')" class="error-window">
    <text class="error-text">*{{error}}</text>
  </view> 

  <!-- Sign up and sign in -->
  <view class="btns"> 
    <button title="Sign up" @press="() => register()" color = "orange"></button>
  </view>

  <view class="btns"> 
    <button title="Sign in" @press="goToLoginScreen" color = "coral"></button>
  </view>

  </view>
</template>

<script>
import { register } from "../FirebaseModules/user-auth"

export default {
  // Declare `navigation` as a prop
  props: {
    navigation: {
      type: Object
    },
    userSignedIn:{
      type : Function
    },
    login:{
      type : Function
    }
  },

  data: function() {
      return {
        email : "",
        password : "",
        error : ""
      };
    },

  methods: {
    goToLoginScreen() {
      this.navigation.navigate("Login");
    },
    goToControlScreen() {
      this.navigation.navigate("Control");
    },
    // Registering function
    register:async function() {
      var registerSuccess = await register(this.email, this.password);

      if (registerSuccess.errorMessage ) {
        if (registerSuccess.errorMessage.substring(0,8) == "Function") {
            this.error = "";
        } else {
          console.log(registerSuccess.errorMessage)
          this.error = registerSuccess.errorMessage; 
      }   
      } else {
          this.goToControlScreen();
      }

    },
       
  },
  // components: { 
  //   header
  // },
}
</script>

<style >


.container {
  background-color:teal;
  height: 100%;
  align-items: center;
  flex: 1;
}

.AppTitle {
  padding-top: 100;
  margin: 50px;
  font-size: 30;
  color:whitesmoke
}

.text-input-container {
   width: 300;
  height: 40;
  border-color: whitesmoke;
  border-width: 2px;
  border-radius: 10px;
  padding-left: 10px;
  padding-right: 10px;
  margin-bottom: 20px;
}

.btns{
  width: 120;
  padding: 10px;
  margin-top: 20px;
  padding-left: 20px;
  padding-right: 20px;
  margin: 10px;
}

.signupBtns{
  width: 120;
  height: 40;
  padding: 10px;
  padding-left: 20px;
  padding-right: 20px;
  margin: 10px;
}

.error-text {
  font-size:18;
  color:white;
}
</style>