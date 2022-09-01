<script>
import moment from "moment";
import axios from "@nuxtjs/axios";

export default {
  data() {
    return {
      roles: ["student", "lecturer", "admin"],
      formValid: true,
      user: { role: "student" },
      momentUse: moment,
      userLists: [],
      nameRules: [
        (name) => !!name || "Name is required",
        (name) =>
          (name && name.length <= 100) ||
          "Name must be less than 100 characters",
      ],
      passwordRules: [
        (p) => !!p || "Password is required",
        (p) =>
          (p && p.length >= 8 && p.length <= 14) ||
          "Password must between 8 and 14",
      ],
      confirmPasswordRules: [
        (p) => !!p || "Password is required",
        (p) => p == this.user.password || "Password not match",
      ],
      emailRules: [
        (email) => !!email || "Email is required",
        (email) => /.+.@.+\..+/.test(email) || "Email must be valid",
      ],
    };
  },
  methods: {
    create() {
      if (this.$refs.form.validate() && this.checkUnique()) {
        this.$axios
          .$post(`/api/users`, {
            name: this.user.name,
            email: this.user.email,
            password: this.user.password,
            role: this.user.role,
          })
          .then(() => {
            this.$router.push("/userlists");
          });
      }
    },
    checkUnique() {
      let nameList = [];
      let emailList = [];
      this.userLists.forEach((u) => {
        nameList.push(u.name);
        emailList.push(u.email);
      });
      return (
        !nameList.includes(this.user.name) &&
        !emailList.includes(this.user.email)
      );
    },
  },
  async created() {
    this.userLists = await this.$axios.$get("/api/users");
  },
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
          <h1 class="tw-mt-10 tw-text-3xl tw-font-semibold">Signup</h1>
          <v-form class="tw-m-10 tw-text-center" ref="form" v-model="formValid">
            <v-text-field
              v-model="user.name"
              label="Name"
              :counter="100"
              :rules="nameRules"
              required
            >
            </v-text-field>
            <v-text-field
              v-model="user.email"
              label="Email"
              :counter="50"
              :rules="emailRules"
              required
            ></v-text-field>
            <v-text-field
              type="password"
              v-model="user.password"
              label="Password"
              :rules="passwordRules"
              required
            /><v-text-field
              type="password"
              v-model="user.comfirmpassword"
              label="Confrimpassword"
              :rules="confirmPasswordRules"
              required
            />
            <v-radio-group v-model="user.role" row>
              <v-radio
                v-for="(n, index) in roles"
                :key="index"
                :label="n"
                :value="n"
                color="black"
              ></v-radio>
            </v-radio-group>
            <v-btn
              class="tw-bg-gradient-to-r tw-from-[#6196FF] tw-to-[#A9B5FF] tw-w-[100%]"
              @click="create"
              ><div class="tw-text-white">Join us</div></v-btn
            >
          </v-form>
        </div>
      </div>
    </div>
  </div>
</template>

<style></style>
