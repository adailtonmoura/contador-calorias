<template>
  <RegisterContainer>
    <header>
      <img :src="'/images/logo.svg'" />
      <h1>Kalorie Kounter</h1>
    </header>
    <div v-if="step == 1">
      <div></div>
      <form @submit.prevent="submit">
        <LineStep v-bind:stepp="step" />
        <div>
          <label for="name">Name</label>
          <input
            id="name"
            type="text"
            v-model="form.name"
            required
            autofocus
            autocomplete="name"
          />
        </div>
        <div>
          <label for="email">Email</label>
          <div>
            <div>
              <img :src="'/images/email.svg'" />
            </div>
            <input id="email" type="email" v-model="form.email" required />
          </div>
        </div>
        <div>
          <label for="password">Password</label>
          <div>
            <div>
              <img :src="'/images/password.svg'" />
            </div>
            <input
              id="password"
              type="password"
              v-model="form.password"
              required
              autocomplete="new-password"
            />
          </div>
        </div>
        <div>
          <label for="password_confirmation">Confirm password</label>
          <div>
            <div>
              <img :src="'/images/password.svg'" />
            </div>
            <input
              id="password_confirmation"
              type="password"
              v-model="form.password_confirmation"
              required
              autocomplete="new-password"
            />
          </div>
        </div>
        <button type="submit">Next</button>
      </form>
    </div>
    <div v-if="step == 2">
      <div></div>
      <form @submit.prevent="finish">
        <LineStep v-bind:stepp="step" />

        <div>
          <label for="height">Your height (cm)</label>
          <input
            id="height"
            type="number"
            v-model="form.height"
            required
            autocomplete="height"
            placeholder="100 cm"
          />
        </div>
        <div>
          <label for="weigth">Your weight (Kg)</label>
          <input
            id="weigth"
            type="number"
            v-model="form.weight"
            required
            placeholder="80 Kg"
            autocomplete="weigth"
          />
        </div>
        <footer>
          <button type="submit">Next</button>
          <button type="button" v-on:click="back">Back</button>
        </footer>
      </form>
    </div>
  </RegisterContainer>
</template>

<script>
import { RegisterContainer } from "../styles/Register.js";
import LineStep from "../../components/LineStep/LineStep";

export default {
  data() {
    return {
      form: this.$inertia.form({
        name: "",
        email: "",
        password: "",
        password_confirmation: "",
        height: 0,
        weight: 0,
      }),
      step: 1,
      message: false,
    };
  },
  components: {
    RegisterContainer,
    LineStep,
  },
  methods: {
    finish() {
        console.log(this.form.password)
        this.form.post(this.route('register'), {
            onFinish: () => this.form.reset('password', 'password_confirmation'),
        })
    },
    submit() {
      if (this.form.password !== this.form.password_confirmation) {
        this.message = true;
      } else {
        this.step += 1;
      }
    },
    next() {
      this.step += 1;
    },
    back() {
      this.step -= 1;
    },
  },
};
</script>
