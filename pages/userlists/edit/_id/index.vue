<script>
import axios from "@nuxtjs/axios";
import moment from "moment";
import { mdiApplicationEditOutline, mdiDelete } from "@mdi/js";
export default {
  data() {
    return {
      roles: ["student", "lecturer", "admin"],
      formValid: true,
      user: {},
      momentUse: moment,
      icon: {
        mdiApplicationEditOutline,
        mdiDelete,
      },
      nameRules: [
        (name) => !!name || "Name is required",
        (name) =>
          (name && name.length <= 100) ||
          "Name must be less than 100 characters",
      ],
      emailRules: [
        (email) => !!email || "Email is required",
        (email) => /.+.@.+\..+/.test(email) || "Email must be valid",
      ],
    };
  },
  methods: {
    async getUser() {
      this.user = await this.$axios.$get(`/api/users/${this.$route.params.id}`);
    },
    edit() {
      if (this.$refs.form.validate()) {
        this.$axios.$put(`/api/users/${this.user.id}`, {
          name: this.user.name,
          email: this.user.email,
          role: this.user.role,
        });
        this.$router.push("/userlists");
      }
    },
  },
  created() {
    this.getUser();
  },
};
</script>

<template>
  <div>
    <div><h1 class="tw-text-5xl tw-font-bold">Edit User</h1></div>

    <div class="tw-grid tw-justify-center">
      <div
        class="tw-h-[50vh] tw-w-[40vw] tw-border tw-border-2 tw-border-t-8 tw-border-[#031B89] tw-mt-10"
      >
        <v-form class="tw-m-10 tw-text-center" ref="form" v-model="formValid">
          <v-text-field
            v-model="user.name"
            label="Name"
            :counter="100"
            :rules="nameRules"
            required
          >
          </v-text-field
          ><v-text-field
            v-model="user.email"
            label="Email"
            :counter="50"
            :rules="emailRules"
            required
          ></v-text-field>
          <v-radio-group v-model="user.role" row>
            <v-radio
              v-for="(n, index) in roles"
              :key="index"
              :label="n"
              :value="n"
              color="black"
            ></v-radio>
          </v-radio-group>
          <v-btn>Cancle</v-btn>
          <v-btn @click="edit">Save</v-btn>
        </v-form>
      </div>
    </div>
    {{ user }}
  </div>
</template>

<style></style>
