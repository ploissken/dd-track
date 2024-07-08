<template>
  <v-sheet class="bg-transparent d-flex ga-2">
    <v-chip
      v-for="view in views"
      @click="$emit('change:view', view.name)"
      :color="viewType === view.name ? 'purple-darken-4' : undefined"
      :variant="viewType === view.name ? 'flat' : undefined"
    >
      {{ view.emoji }}
    </v-chip>
  </v-sheet>
</template>

<script setup lang="ts">
defineProps({
  viewType: String,
});

const views = [
  {
    emoji: "📆",
    name: "calendar",
  },
  {
    emoji: "📜",
    name: "table",
  },
];
</script>
