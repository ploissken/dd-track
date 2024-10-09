<template>
  <v-toolbar density="compact">
    <template v-slot:prepend>
      <div class="text-h6">👩🏽‍❤️‍👨🏼 d&d</div>
    </template>
    <v-tabs color="white">
      <RouterLink v-for="link in menuItems" :to="link.destiny">
        <v-tab>{{ link.emoji }}</v-tab>
      </RouterLink>
    </v-tabs>
  </v-toolbar>
</template>

<script setup lang="ts">
const menuItems = [
  { destiny: "/", emoji: "🏠" },
  { destiny: "/fix", emoji: "🔧" },
];
</script>
