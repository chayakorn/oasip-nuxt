<script>
import axios from "@nuxtjs/axios";
import moment from "moment";
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
    };
  },
  methods: {
    getUser() {},
  },
  async mounted() {
    try {
      this.userLists = await this.$axios.$get("/api/users");
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
    <div>
      <h1 class="tw-text-5xl tw-font-bold">Users</h1>
      <p>{{ userLists.length }} users</p>
    </div>

    <div v-if="!empty">
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
            <v-icon large>{{ icon.mdiDelete }}</v-icon>
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
