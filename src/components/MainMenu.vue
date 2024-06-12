<template>
  <v-toolbar density="compact">
    <template v-slot:prepend>
      <div class="text-h6">👩🏽‍❤️‍👨🏼 d&d</div>
    </template>
    <v-tabs color="white">
      <RouterLink v-for="link in menuItems" :to="link.destiny">
        <v-tab :disabled="link.disabled">{{ link.emoji }}</v-tab>
      </RouterLink>
    </v-tabs>
  </v-toolbar>
</template>

<script setup lang="ts">
const menuItems = [
  { destiny: "/", emoji: "📆" },
  { destiny: "/create-task", emoji: "✏️" },
  { destiny: "/history-view", emoji: "📜" },
  { destiny: "/", emoji: "📊", disabled: true },
  { destiny: "/fix", emoji: "🔧" },
];
</script>
