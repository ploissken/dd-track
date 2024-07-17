<template>
  <v-container class="fill-height p-0 m-0 d-flex align-start justify-start">
    <v-row class="justify-center">
      <v-col cols="12">
        <v-calendar
          class="bg-transparent"
          ref="calendar"
          v-model="initialDate"
          hide-header
          :events="calendarEvents"
          :view-mode="type"
          :weekdays="weekday"
        />
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup lang="ts">
import { ref } from "vue";
import { CalendarEvent } from "../../interfaces/CalendarEvent";

defineProps({
  loading: Boolean,
  currentPersonFilter: String,
  calendarEvents: Array<CalendarEvent>,
  currentChallengeFilter: String,
  currentTaskFilter: {
    type: String,
    default: "",
  },
});

const type = ref("month");
const weekday = ref([0, 1, 2, 3, 4, 5, 6]);
const initialDate = ref([new Date()]);
</script>
