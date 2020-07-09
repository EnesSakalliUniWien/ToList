<template>
  <v-app>
    <ToDoList></ToDoList>
  </v-app>
</template>

<script lang="ts">
import Vue from "vue";
import ToDoList from "./components/ToDoList.vue";

export default Vue.extend({
  name: "App",

  components: {
    ToDoList,
  },

  data: () => ({
    //
  }),
});
</script>
