<script>
import moment from "moment";
import axios from "@nuxtjs/axios";

export default {
  data() {
    return {
      formValid: true,
      user: {},
      momentUse: moment,
      passwordRules: [(p) => !!p || "Password is required"],
      emailRules: [
        (email) => !!email || "Email is required",
        (email) => /.+.@.+\..+/.test(email) || "Email must be valid",
      ],
    };
  },
  methods: {
    login() {
      if (this.$refs.form.validate()) {
        this.$axios
          .$post(`/api/login`, {
            email: this.user.email,
            password: this.user.password,
          })
          .then((res) => {
            localStorage.setItem("token", res.token);
            this.$router.push("/userlists");
          });
      }
    },
  },
  async created() {},
};
</script>

<template>
  <div>
    <div class="tw-grid tw-justify-center">
      <div
        class="tw-w-[50vw] tw-mt-10 tw-grid tw-grid-cols-2 tw-rounded-2xl tw-bg-white tw-drop-shadow-lg"
      >
        <div
          class="tw-bg-gradient-to-r tw-from-[#6196FF] tw-to-[#A9B5FF] tw-rounded-2xl tw-drop-shadow-lg tw-text-center"
        >
          <div class="tw-mt-10">
            <p class="tw-text-3xl tw-font-bold tw-text-white">
              Glad to see you
            </p>
            <p class="tw-text-white tw-text-lg">
              Welcome to <span class="tw-text-[#031B89]">OASIP T2P</span>
            </p>
            <p class="tw-text-white">Let's us help you schedule your events!</p>
          </div>
          <div class="tw-grid tw-place-items-center">
            <img
              src="../assets/images/Microsites-amico.png"
              alt=""
              class="tw-w-[70%]"
            />
          </div>
        </div>
        <div class="tw-bg-white tw-text-center">
          <h1 class="tw-mt-10 tw-text-3xl tw-font-semibold">Signin</h1>
          <v-form class="tw-m-10 tw-text-center" ref="form" v-model="formValid">
            <v-text-field
              v-model="user.email"
              label="Email"
              :rules="emailRules"
              required
            ></v-text-field>
            <v-text-field
              type="password"
              v-model="user.password"
              label="Password"
              :rules="passwordRules"
              required
            />
            <v-btn
              class="tw-bg-gradient-to-r tw-from-[#6196FF] tw-to-[#A9B5FF] tw-w-[100%]"
              @click="login"
              ><div class="tw-text-white">Join us</div></v-btn
            >
          </v-form>
        </div>
      </div>
    </div>
  </div>
</template>

<style></style>
