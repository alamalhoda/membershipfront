<script setup>
import PostsTable from "./components/PostsTable.vue";
import MembershipCode from "./components/MembershipCode.vue";
</script>

<template>
  <MembershipCode></MembershipCode>
  <posts-table></posts-table>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
