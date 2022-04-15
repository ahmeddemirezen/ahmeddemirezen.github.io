<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="toggleLeftDrawer"
        />

        <q-toolbar-title> Ahmed Demirezen </q-toolbar-title>

        <div>{{ time }}</div>
      </q-toolbar>
    </q-header>

    <q-drawer v-model="leftDrawerOpen" show-if-above bordered>
      <q-list>
        <q-item-label header> Essential Links </q-item-label>

        <EssentialLink
          v-for="link in essentialLinks"
          :key="link.title"
          v-bind="link"
        />
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import { defineComponent, ref } from "vue";
import EssentialLink from "components/EssentialLink.vue";

const linksList = [
  {
    title: "Likedin",
    caption: "Ahmed Demirezen",
    icon: "account_box",
    link: "https://www.linkedin.com/in/ahmeddemirezen/",
  },
  {
    title: "Github",
    caption: "github.com/ahmeddemirezen",
    icon: "code",
    link: "https://github.com/ahmeddemirezen",
  },
  {
    title: "Discord",
    caption: "FeezX#0619",
    icon: "chat",
    link: "",
  },
  {
    title: "Twitter",
    caption: "@demirezenahmed",
    icon: "rss_feed",
    link: "https://twitter.com/demirezenahmed",
  },
  {
    title: "Wide Game Studio",
    caption: "WGS",
    icon: "public",
    link: "https://www.widegamestudio.com/",
  },
  {
    title: "Instagram",
    caption: "@ahmeddemirezen",
    icon: "tag",
    link: "https://www.instagram.com/ahmeddemirezen/",
  },
];

export default defineComponent({
  name: "MainLayout",

  components: {
    EssentialLink,
  },
  mounted() {
    setInterval(() => {
      this.time = new Date().toLocaleTimeString();
    }, 1000);
  },
  data() {
    return {
      time: "",
    };
  },

  setup() {
    const leftDrawerOpen = ref(false);

    return {
      essentialLinks: linksList,
      leftDrawerOpen,
      toggleLeftDrawer() {
        leftDrawerOpen.value = !leftDrawerOpen.value;
      },
    };
  },
});
</script>
