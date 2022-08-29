<template>
  <v-app dark>
    <v-navigation-drawer
      v-model="drawer"
      :mini-variant="miniVariant"
      :clipped="true"
      right
      fixed
      app
    >
      <v-list>
        <v-list-item
          v-for="(item, i) in items"
          :key="i"
          :to="item.to"
          router
          exact
        >
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="item.title" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar :clipped-right="true" elevation="4" fixed app height="70">
      <v-img src="./icon.png" max-width="100"></v-img>
      <v-toolbar-title v-text="title" />
      <v-spacer></v-spacer>

      <v-btn nuxt to="/" text>HOME</v-btn>
      <v-btn nuxt to="/about" text>ABOUT</v-btn>
      <v-menu open-on-hover offset-y flat>
        <!-- <button>LIST</button> -->
        <template v-slot:activator="{ on, attrs }">
          <v-btn v-bind="attrs" v-on="on" text>LIST</v-btn>
        </template>
        <v-list>
          <v-list-item>
            <v-btn text nuxt to="/userlists">Users</v-btn>
          </v-list-item>
          <v-list-item>
            <v-btn text to="eventlists">Events</v-btn>
          </v-list-item>
        </v-list></v-menu
      >
      <v-btn nuxt to="/category" text>categories</v-btn>
      <v-btn nuxt to="/signup" text>signup</v-btn>

      <!-- <v-toolbar-title v-text="tile"></v-toolbar-title> -->
      <v-btn icon @click.stop="drawer = !drawer">
        <v-icon>mdi-menu</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <v-container fluid>
        <Nuxt />
      </v-container>
    </v-main>
    <v-footer :absolute="!fixed" app>
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  name: "DefaultLayout",
  data() {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          title: "HOME",
          to: "/",
        },
        {
          title: "ABOUT",
          to: "/about",
        },
        {
          title: "LIST",
          to: "",
        },
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: "OASIP US-1",
    };
  },
};
</script>
<style>
html {
  scroll-behavior: smooth;
}
</style>
