<template>
  <v-sheet class="bg-transparent d-flex ga-2">
    <v-chip
      v-for="user in allUsers"
      @click="$emit('changePerson', user.id)"
      :color="currentPersonFilter === user.id ? 'purple-darken-4' : undefined"
      :variant="currentPersonFilter === user.id ? 'flat' : undefined"
    >
      {{ user.label }}
    </v-chip>
  </v-sheet>
</template>

<script setup lang="ts">
import users from "../../assets/users.json";

defineProps({
  currentPersonFilter: String,
});

const allUsers = [{ label: "👩🏽‍🌾👨🏼‍💻", id: "" }, ...users];
</script>
