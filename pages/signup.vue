<script setup>
import { reactive } from "vue";
import { createClient } from "@supabase/supabase-js/dist/main/index.js";

const { supabaseUrl, supabasePublicKey } = useRuntimeConfig();
const supabase = createClient(supabaseUrl, supabasePublicKey);

const loginState = reactive({
  email: "",
  password: "",
  user: null,
  error: null,
});

const signUp = async () => {
  let { user, error } = await supabase.auth.signUp({
    email: loginState.email,
    password: loginState.password,
  });

  loginState.user = user;
  loginState.error = error;
};
</script>

<template>
  <div>
    <h1>Sign up</h1>
    <pre>{{ loginState }}</pre>
    <form @submit.prevent>
      <div>
        <label for="email">Email</label>
        <input
          type="email"
          v-model="loginState.email"
          id="email"
          placeholder="email"
        />
      </div>
      <div>
        <label for="password">Password</label>
        <input type="password" v-model="loginState.password" id="password" />
      </div>
      <div>
        <button @click="signUp">Sign Up</button>
      </div>
    </form>
  </div>
</template>
