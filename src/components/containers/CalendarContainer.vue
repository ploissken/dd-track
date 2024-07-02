<template>
  <v-container class="fill-height p-0 m-0 d-flex align-start justify-start">
    <v-row class="justify-center">
      <v-col cols="12">
        <v-calendar
          class="bg-transparent"
          ref="calendar"
          v-model="initialDate"
          :events="filteredEvents"
          :view-mode="type"
          :weekdays="weekday"
        />
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup lang="ts">
import { ref } from "vue";

export interface CalendarEvent {
  title: String;
  start: Date;
  end: Date;
  color: String;
}
defineProps({
  loading: Boolean,
  currentPersonFilter: String,
  filteredEvents: Array<CalendarEvent>,
  currentTaskFilter: {
    type: String,
    default: "",
  },
  currentChallengeFilter: String,
});

const type = ref("month");
const weekday = ref([0, 1, 2, 3, 4, 5, 6]);
const initialDate = ref([new Date()]);
</script>
