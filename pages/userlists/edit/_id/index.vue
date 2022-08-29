<script>
import axios from "@nuxtjs/axios";
import moment from "moment";
import { mdiApplicationEditOutline, mdiDelete } from "@mdi/js";
export default {
  data() {
    return {
      user: {},
      momentUse: moment,
      icon: {
        mdiApplicationEditOutline,
        mdiDelete,
      },
    };
  },
  async asyncData({ $axios }) {
    let { data } = await $axios.get("/api/users/" + this.$route.params.id);
    return { user: data };
  },
};
</script>

<template>
  <div>
    {{ $route.params.id }}
  </div>
</template>

<style></style>
