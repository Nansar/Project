<template>
  <nav class="menu">

    <router-link to="/" class="linkBarra"> Home </router-link>

    <router-link to="/account" class="linkBarra">Your Account</router-link>

    <router-link to="/" class="linkBarra">Your Tasks</router-link>


    <div class="">
      <ul>
        <li class="log-out-welcome">
          <p class="linkBarra">Welcome,  
            <span class="email">{{ getEmailPrefix(getUser.email) }}</span></p>
        </li>
        <li>
          <button @click="signOut" class="logOut">Log Out</button>
        </li>
      </ul>
    </div>
  </nav>
</template>

<script setup>

import { useUserStore } from "../stores/user";
import { computed } from "vue";
import { useRouter } from "vue-router";
import { ref } from 'vue';
import { supabase } from "../supabase";

const user = supabase.auth.user();

const route = "/";
const buttonText = "Todo app";


const getUser = useUserStore().user;
console.log(useUserStore().user);

const userEmail = getUser.email;

const userStore = useUserStore();
const router = useRouter();

const getUserEmail = () => {
  const user = getUser.value;
  if (user) {
    userEmail.value = user.email;
  }
};
getUserEmail();

const redirect = useRouter();

const signOut = async () => {
  try{
   

    await useUserStore().signOut();
    redirect.push({ path: "/auth/login" });

  } catch (error) {}
};

const getEmailPrefix = (email) => {
  const atIndex = email.indexOf('@');
  if (atIndex !== -1) {
    return email.slice(0, atIndex);
  }
  return email;
};
</script>

<style scoped>



nav {
  background-color: rgb(48, 115, 170);
  display: flex;
  width: 100%;
  justify-content: space-around;
  align-items: center;
}

nav ul {
  list-style: none;
  padding-inline-start: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.linkBarra{
  color: rgb(87, 15, 15);
  text-decoration: none;
  font-size: 3vh;
  text-decoration: none;
  background-color: transparent;
  
}

.email{
  color: rgb(87, 15, 15);
  background-color: transparent;
}

.logOut {

  height: 45px;
  width: 80px;
  align-content: center;
  padding: 10px 15px;
  background-color: rgb(128, 134, 146);
  border: none;
  border-radius: 5px;
  cursor: pointer;
  
}
</style>