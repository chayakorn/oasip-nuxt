<script>
import axios from "@nuxtjs/axios";
import moment from "moment";
import Swal from "sweetalert2";
import { mdiApplicationEditOutline, mdiDelete } from "@mdi/js";
export default {
  data() {
    return {
      empty: false,
      userLists: [],
      momentUse: moment,
      icon: {
        mdiApplicationEditOutline,
        mdiDelete,
      },
      alt: Swal,
    };
  },
  methods: {
    async deleteUser(id) {
      this.alt
        .fire({
          title: "Are you sure?",
          text: "You won't be able to revert this!",
          icon: "warning",
          showCancelButton: true,
          confirmButtonColor: "#3085d6",
          cancelButtonColor: "#d33",
          confirmButtonText: "Yes, delete it!",
        })
        .then((result) => {
          if (result.isConfirmed) {
            this.$axios
              .$delete(`/api/users/${id}`, {
                headers: {
                  Authorization: `US1 ${localStorage.getItem("token")}`,
                },
              })
              .then(() => {
                this.getUserList();
              });

            Swal.fire("Deleted!", "Your file has been deleted.", "success");
          }
        });
    },
    async getUserList() {
      this.userLists = await this.$axios
        .$get("/api/users", {
          headers: { Authorization: `US1 ${localStorage.getItem("token")}` },
        })
        .catch((e) => {
          this.$router.push("/signin");
        });
    },
  },
  async mounted() {
    try {
      this.getUserList();
    } catch (e) {
      this.empty = true;
    }
  },
  // async asyncData({ $axios }) {
  //   let { data } = await $axios.get("/api/users");
  //   return { userLists: data };
  // },
};
</script>

<template>
  <div class="tw-m-5">
    <div class="tw-grid tw-grid-cols-2">
      <div>
        <h1 class="tw-text-5xl tw-font-bold">Users</h1>
        <p>{{ userLists.length }} users</p>
      </div>
      <div class="tw-grid tw-place-content-end">
        <v-btn class="mx-2" fab dark color="indigo" nuxt to="/createuser">
          <v-icon dark> mdi-plus </v-icon>
        </v-btn>
      </div>
    </div>

    <div class="tw-mt-5" v-if="!empty">
      <v-card v-for="(user, index) in userLists" :key="index">
        <div class="tw-flex tw-flex-row tw-m-4 tw-text-center tw-items-center">
          <div
            class="tw-h-20 tw-w-20 tw-rounded-full tw-bg-slate-500 tw-mr-10"
          ></div>
          <div class="tw-flex-auto tw-flex-col tw-break-all tw-w-28">
            <p>
              {{ user.name }}
            </p>
            <p>
              {{ user.email }}
            </p>
          </div>
          <div class="tw-flex-auto">
            <v-chip
              :color="
                user.role == 'student'
                  ? '#F4D6DC'
                  : user.role == 'lecturer'
                  ? '#DDEDEB'
                  : '#F8EED0'
              "
              :text-color="
                user.role == 'student'
                  ? '#E54F6D'
                  : user.role == 'lecturer'
                  ? '#74C4BA'
                  : '#F8C630'
              "
              >{{ user.role }}</v-chip
            >
          </div>
          <div class="tw-flex-auto">
            <p>{{ momentUse(user.createdOn).format("YYYY/MM/DD") }}</p>
            <p>{{ momentUse(user.updatedOn).format("hh:mm:ss") }}</p>
          </div>
          <div class="tw-flex-auto">
            <p>{{ momentUse(user.updatedOn).format("YYYY/MM/DD") }}</p>
            <p>{{ momentUse(user.updatedOn).format("hh:mm:ss") }}</p>
          </div>
          <div class="tw-flex-auto tw-space-x-3">
            <nuxt-link :to="'/userlists/edit/' + user.id"
              ><v-icon large>{{
                icon.mdiApplicationEditOutline
              }}</v-icon></nuxt-link
            >
            <v-icon large @click="deleteUser(user.id)">{{
              icon.mdiDelete
            }}</v-icon>
          </div>
        </div>
      </v-card>
    </div>
    <div class="text-center" v-if="empty">
      <h1 class="tw-text-5xl tw-font-bold tw-text-gray-400">No Users</h1>
    </div>
  </div>
</template>

<style></style>
