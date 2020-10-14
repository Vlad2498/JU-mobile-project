<template>
  <view class="container">

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
    <text class="error-text">{{error}}</text>
  </view> 

  <!-- Sign up and sign in -->
  <view class="btns"> 
    <button title="Sign in" @press="() => signin()" color = "coral"></button>
  </view>

  <view class="signupBtns"> 
    <button title="Sign up" @press="goToSignUpScreen" color = "orange"></button>
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
  background-color:teal;
  align-items: center;
  flex: 1;
}

.AppTitle {
  padding-top: 100;
  margin: 50px;
  font-size: 30;
  color:whitesmoke;
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