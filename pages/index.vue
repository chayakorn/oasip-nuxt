<template>
  <div>
    <v-carousel cycle interval="6000" progress class="tw-mt-10">
      <v-carousel-item v-for="(cate, index) in categories" :key="index">
        <div class="tw-grid tw-grid-cols-2">
          <div>
            <v-img :src="cate.image" alt="" srcset="" />
          </div>
          <div class="tw-flex justify-center tw-mt-6">
            <div class="tw-text-center tw-grid tw-row-auto">
              <h1 class="tw-font-bold tw-text-[2vw] tw-text-black">
                Online Apointment Scheduling System
              </h1>
              <h1 class="tw-font-bold tw-text-[3vw] tw-text-[#031B89]">
                {{ cate.eventCategoryName }}
              </h1>
              <h1 class="tw-font-bold tw-text-[2vw] tw-text-[#6196FF]">
                ━━ clinic
              </h1>
              <div class="tw-row-span-5 tw-flex tw-justify-center">
                <v-btn>More details</v-btn>
              </div>
            </div>
          </div>
        </div>
      </v-carousel-item>
    </v-carousel>
    <v-card
      elevation="1"
      v-for="(cate, index) in categories"
      :key="index"
      class="tw-m-10 tw-relative"
      ><div class="tw-grid tw-grid-cols-4">
        <v-img :src="cate.image" max-width="400" alt="" />
        <div class="tw-col-span-2">
          <v-card-title class="">{{ cate.eventCategoryName }}</v-card-title>
          <v-card-text>{{
            cate.eventCategoryDescription == ""
              ? "No description"
              : cate.eventCategoryDescription
          }}</v-card-text>
        </div>
        <div
          class="tw-grid tw-justify-center tw-align-items-center tw-text-center"
        >
          <div>{{ cate.eventDuration }}<br />minutes</div>
        </div>
      </div>
      <v-btn absolute right bottom large>BOOK</v-btn>
    </v-card>
  </div>
</template>

<style></style>
<script>
import axios from "@nuxtjs/axios";
export default {
  data() {
    return {
      categories: [],
    };
  },
  async created() {
    this.categories = await this.$axios.$get("/api/event-categories");
  },
};
</script>
