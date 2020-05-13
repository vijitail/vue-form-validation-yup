<template>
  <div id="app">
    <form class="login-form" @submit.prevent="loginUser">
      <h2>Login</h2>
      <form-input
        label="Email"
        v-model="values.email"
        type="email"
        @validate="validate('email')"
        name="email"
        :error="errors.email"
      ></form-input>
      <form-input
        label="Password"
        v-model="values.password"
        type="password"
        @validate="validate('password')"
        name="password"
        :error="errors.password"
      ></form-input>
      <button class="btn btn-primary btn-block">Login</button>
    </form>
  </div>
</template>

<script>
import { object, string, email } from "yup";

import FormInput from "./components/FormInput";

const loginFormSchema = object().shape({
  email: string()
    .email()
    .required(),
  password: string().required()
});

export default {
  name: "app",
  components: {
    "form-input": FormInput
  },
  data() {
    return {
      values: {
        email: "",
        password: ""
      },
      errors: {
        email: "",
        password: ""
      }
    };
  },
  methods: {
    loginUser() {
      loginFormSchema
        .validate(this.$data.values, { abortEarly: false })
        .then()
        .catch(err => {
          err.inner.forEach(error => {
            this.errors = { ...this.errors, [error.path]: error.message };
          });
        });
    },
    validate(field) {
      loginFormSchema
        .validateAt(field, this.$data.values)
        .then(() => {
          this.$data.errors[field] = "";
        })
        .catch(err => {
          this.errors = { ...this.errors, [err.path]: err.message };
        });
    }
  }
};
</script>

<style lang="scss">
#app {
  display: flex;
  min-height: 100vh;
  justify-content: center;
  align-items: center;
  padding: 16px;

  .login-form {
    max-width: 400px;
    width: 100%;
  }
}
</style>
