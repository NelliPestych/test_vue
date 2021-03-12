<template>
  <div class="container mt-4 users">
    <div class="row">
      <div class="col-sm-6 mx-auto">
        <form @submit.prevent="greeting" novalidate>
          <div v-if="login === false" class="login">
            <div class="mb-3">
              <label for="exampleInputEmail1" class="form-label">Login</label>
              <input
                @blur="$v.formReg.exampleInputEmail1.$touch()"
                :class="{ 'is-invalid': $v.formReg.exampleInputEmail1.$error }"
                v-model="formReg.exampleInputEmail1"
                type="email"
                class="form-control"
                id="exampleInputEmail1"
                aria-describedby="emailHelp"
              />

              <div
                v-if="!$v.formReg.exampleInputEmail1.$required"
                class="invalid-feedback"
              >
                ERROR! Please enter your LOGIN
              </div>
            </div>
            <div class="mb-3">
              <label for="exampleInputPassword1" class="form-label"
                >Password</label
              >
              <input
                @blur="$v.formReg.exampleInputPassword1.$touch()"
                :class="{
                  'is-invalid': $v.formReg.exampleInputPassword1.$error
                }"
                v-model="formReg.exampleInputPassword1"
                type="password"
                class="form-control"
                id="exampleInputPassword1"
              />

              <div
                v-if="!$v.formReg.exampleInputPassword1.$required"
                class="invalid-feedback"
              >
                ERROR! Please enter your PASSWORD
              </div>
              <div v-if="input === false">
                <h3>
                  OOPS! YOU DIDN'T ENTER YOUR LOGIN OR PASSWORD! FILL IN THE
                  FORM, PLEASE.
                </h3>
              </div>
            </div>

            <button @click="register" type="submit" class="btn btn-primary">
              Submit
            </button>
          </div>
          <div v-if="login === true" class="login">
            <h2>Welcome to your personal account!</h2>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import { required } from "vuelidate/lib/validators";
export default {
name: "users",
  data() {
    return {
      input: true,
      login: false,
      formReg: {
        exampleInputEmail1: "",
        exampleInputPassword1: ""
      }
    };
  },

methods: {
  greeting() {
    if (this.formReg.exampleInputEmail1 && this.formReg.exampleInputPassword1) {
      this.login = true;
      console.log("Регистрация прошла успешно!");
      console.log(
      fetch("http://localhost:3000/users", {
        method: "POST",
        headers: {
          Accept: "application/json",
          "Content-Type": "application/json"
        },
        body: JSON.stringify({
          email: this.formReg.exampleInputEmail1,
          password: this.formReg.exampleInputPassword1
        })
      }).then(function(response) {
        return response.json();
      }));
    } else {
      this.input = false;
    }
  },
  register() {
    console.log(this.formReg.exampleInputEmail1);
    console.log(this.formReg.exampleInputPassword1);
  }
},
  validations: {
  formReg: {
    exampleInputEmail1: {
      required,
    },
    exampleInputPassword1: {
      required,
    }
  }
},
}

</script>

<style></style>
