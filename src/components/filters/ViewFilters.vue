<template>
  <v-sheet class="bg-transparent d-flex ga-2">
    <v-chip
      @click="$emit('change:view', 'calendar')"
      :color="viewType === 'calendar' ? 'purple-darken-4' : undefined"
      :variant="viewType === 'calendar' ? 'flat' : undefined"
    >
      📆
    </v-chip>
    <v-chip
      @click="$emit('change:view', 'table')"
      :color="viewType === 'table' ? 'purple-darken-4' : undefined"
      :variant="viewType === 'table' ? 'flat' : undefined"
    >
      📜
    </v-chip>
  </v-sheet>
</template>

<script setup lang="ts">
const props = defineProps({
  viewType: String,
});
console.log(props.viewType);
</script>
