 
<template>
  <div class="background">
  <div class="container">
    <div class="welcome">
    <h3 class="header-title">Log In to ToDo App</h3>
    <p class="header-subtitle">All your task in ToDo App</p>

  

  </div>

<form @submit.prevent="signIn" class="form-sign-in">
      <div class="form">
        <div class="form-input">
          <label class="input-field-label"></label>
          <input
            type="email"
            class="input-field"
            placeholder="example@gmail.com"
            v-model="email"
            required
          />
        </div>
        <div class="form-input" id="pass">
          <label class="input-field-label">
          </label>
     
            <input
              :type="passwordVisible ? 'text' : 'password'"
              class="input-field input-password"
              placeholder="Confirm Password"
              v-model="password"
              required
              
            />
            <span class="password" @click="togglePasswordVisibility('password')">
              <i class="fa" :class="passwordVisible ? 'fa-eye-slash' : 'fa-eye'"></i>
            </span>
        </div>
        <button class="button" type="submit">Sign In</button>
      </div>
    </form>

    <p class="dontAccount">Don't have an account? 
      <router-link to="/auth/signup">Sign Up</router-link></p>
  </div>
</div>
</template>

<script setup>
import PersonalRouter from "./PersonalRouter.vue";
import { useRouter } from "vue-router";
import { useUserStore } from "../stores/user";
import { ref, computed } from "vue";
import { supabase } from "../supabase";
import { storeToRefs } from "pinia";


//Route Variables
const route = "/auth/signup";
const buttonText = "Sign Up";

const email = ref("");
const password = ref("");
const redirect = useRouter();
const userStore = useUserStore();
const errorMsg = ref("");


// Arrow function to Signin user to supaBase
const signIn = async () => {
  try {
    await userStore.signIn(email.value, password.value);
    redirect.push("/");
  } catch (error) {
    errorMsg.value = `Error: ${error.message}`;
    setTimeout(() => {
      errorMsg.value = null;
    }, 2500);
  }
};

const passwordVisible = ref(false);
const confirmPasswordVisible = ref(false);

const togglePasswordVisibility = (field) => {
  if (field === "password") {
    passwordVisible.value = !passwordVisible.value;
  } else if (field === "confirmPassword") {
    confirmPasswordVisible.value = !confirmPasswordVisible.value;
  }
};
</script>

<style>
p {
  text-align: center;
  margin-top: 10px;
}
.dontAccount{
  color:white;
} 


.background{
  height: 100vh;
  width: 100%;
  text-align: center;
  position: relative;
  background-repeat: no-repeat;
  background-size: cover;
}

.input-pasword{
  position: relative;
  padding-right: 35px;
}
.container{
  width: 100%;
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
}
.togglePasswordVisibility{
  flex-wrap: wrap;
  color: #467ffa;
  position: fixed;
  top: 55%;
  right: 700px; 
  transform: translateY(-50%);
  cursor: pointer;
  z-index: 1; 
}

.header-title{
  color: rgb(57, 74, 181);
  text-align: center;
  margin-bottom: 30px;
  font-size: 45px;
}

.header-subtitle{
  color: white;
  text-align: center;
  margin-bottom: 10px;
  font-size: 18px;
}

.form-sign-in {
   padding:30px 30px 30px 30px; 
   width:70%;
}
.form{
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
}

.form-input {
  margin-bottom: 8px;
  width: 80%;
}

.input-field {
  width: 100%;
  padding: 15px;
  border: 1px solid white;
  border-radius: 5px;
}

.button {
  padding: 10px 30px;
  background-color: rgb(51, 73, 185);
  color: black;
  border: none;
  border-radius: 7px;
  cursor: pointer;
  width: 80%;
}
.form{
  width:100%;
}

#pass{
  display:inline;
  align-items: center;
}
.password{
  position: relative;
  top:5%;
  left:50px;
}

</style>