<template>
  <view class="container">

  <view>
      
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
    <text class="error-text">{{error}}</text>
  </view> 

  <!-- Sign up and sign in -->
  <view class="btns"> 
    <button title="Sign in" @press="() => signin()" color = "purple"></button>
  </view>

  <view class="btns"> 
    <button title="Sign up" @press="goToSignUpScreen"></button>
  </view>
  </view>
</template>

<script>

// import { app } from 'firebase'
import {signIn} from "../FirebaseModules/user-auth"



export default {
    data:function() {
    return {
      email:'',
      password:'',
      error:''
    }
  },

  // Declare `navigation` as a prop
  props: {
    navigation: {
      type: Object
    },
  },
  methods: {
    
    goToSignUpScreen() {
      this.navigation.navigate("Signup");
    },
    goToControlScreen() {
      this.navigation.navigate("Control");
    },

// signing async call to the signin function present in user-auth.js file
    signin:async function() {
      var signedIn = await signIn(this.email,this.password);
      if(signedIn.errorMessage) {
      console.log(signedIn.errorMessage)
      console.log("sign in failed")
        this.error = signedIn.errorMessage;
      } else {
        this.error = '';
        // console.log(signedIn);
        console.log("sign in succeded")
        this.goToControlScreen();
      }
    }    

  },
}
</script>




<style >

.container {
  background-color: #0a84ff;
  height: 100%;
  align-items: center;
  flex: 1;
}

.text-input-container {
  width: 300;
  height: 40;
  margin: 10;
  padding-left: 5;
  font-size: 22;
  border-color: whitesmoke;
  border-width: 2px;
  border-radius: 5;
  color:white;
}

.btns{
  margin: 10;
  width: 100px;
}

.error-text {
  font-size:18;
  color:white;
}
</style>