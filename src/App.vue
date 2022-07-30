<template>
  <div class="pt-20 text-white">
    <div class="max-w-screen-sm mx-auto bg-[#2D3741] p-10 rounded-md shadow-xl">
      <h1 class="mb-4 text-3xl">Create Account</h1>
      <form @submit.prevent="submitForm" class="flex flex-col gap-6">
        <base-input
          v-model="formData.username"
          label="Username"
          placeholder="@username"
        />
        <span v-if="v$.username.$error" class="text-red-500 text-sm">
          {{ v$.username.$errors[1].$message }}
        </span>
        <base-input
          v-model="formData.email"
          label="Email"
          placeholder="Email"
        />
        <span
          v-for="error in v$.email.$errors"
          :key="error.$uid"
          class="text-red-500 text-sm"
        >
          {{ error.$message }}
        </span>
        <base-input
          v-model="formData.password"
          label="Password"
          placeholder="Enter password"
        />
        <span
          v-for="error in v$.password.$errors"
          :key="error.$uid"
          class="text-red-500 text-sm"
        >
          {{ error.$message }}
        </span>
        <base-input
          v-model="formData.confirmPassword"
          label="Confirm Password"
          placeholder="Confirm password"
        />
        <span
          v-for="error in v$.confirmPassword.$errors"
          :key="error.$uid"
          class="text-red-500 text-sm"
        >
          {{ error.$message }}
        </span>
        <button
          type="submit"
          class="self-start bg-[#476583] px-3 py-2 rounded-md"
        >
          Create an account
        </button>
      </form>
    </div>
  </div>
</template>

<script setup>
import { computed, reactive } from "vue";
import BaseInput from "./components/BaseInput.vue";
import useVuelidate from "@vuelidate/core";
import {
  required,
  minLength,
  email,
  sameAs,
  helpers,
} from "@vuelidate/validators";

const containUser = (value) => {
  return value.includes("@");
};

const formData = reactive({
  username: "",
  email: "",
  password: "",
  confirmPassword: "",
});

const rules = computed(() => {
  return {
    username: {
      required,
      minLength: minLength(6),
      containUser: helpers.withMessage(
        "The field must contain valid username",
        containUser
      ),
    },
    email: { required, email },
    password: { required },
    confirmPassword: { required, sameAs: sameAs(formData.password) },
  };
});

const v$ = useVuelidate(rules, formData);

const submitForm = async () => {
  const result = await v$.value.$validate();
  if (result) {
    alert("Sucess, form submitted!");
  } else {
    alert("Error, please fill out fields correctly!");
  }
};
</script>

<style>
#app {
  max-width: 640px;
  margin: 60px auto;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
button:hover {
  background-color: #577ca0;
}
</style>
