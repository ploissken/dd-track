<template>
  <v-container class="fill-height p-0 m-0 d-flex align-start justify-start">
    <v-row class="justify-center">
      <v-col cols="12" class="d-flex justify-space-between">
        <v-btn
          @click="() => handleChangeDate(-1)"
          variant="text"
          rounded
          :ripple="false"
          size="large"
        >
          ⬅️
        </v-btn>

        <h2>{{ dateLabel }}</h2>
        <v-btn
          @click="() => handleChangeDate(1)"
          variant="text"
          rounded
          :ripple="false"
          size="large"
        >
          ➡️
        </v-btn>
      </v-col>

      <v-col cols="12">
        <v-calendar
          class="bg-transparent"
          ref="calendar"
          v-model="currentDate"
          hide-header
          :events="calendarEvents"
          :view-mode="mobile ? 'day' : 'month'"
          :weekdays="weekday"
        />
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup lang="ts">
import { ref } from "vue";
import { CalendarEvent } from "../../interfaces/CalendarEvent";
import { useDate, useDisplay } from "vuetify";

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
const { mobile } = useDisplay();
const date = useDate();
const weekday = ref([0, 1, 2, 3, 4, 5, 6]);
const currentDate = ref([new Date()]);
const dateLabel = ref(date.format(currentDate, "month"));

const handleChangeDate = (diff: number) => {
  currentDate.value = [
    mobile.value
      ? new Date(date.addDays(currentDate.value, diff) as string)
      : new Date(date.addMonths(currentDate.value, diff) as string),
  ];
  dateLabel.value = date.format(
    new Date(currentDate.value as unknown as string),
    "month"
  );
};
</script>

<style>
.v-chip.v-chip--size-default {
  font-size: 0.6rem;
}

.v-chip.v-chip--density-comfortable {
  height: auto;
  width: calc(100% - 1em);
  margin: 0.5em;
  padding-top: 0.5em;
  padding-bottom: 0.5em;
}

.v-chip__content {
  text-wrap: wrap;
}
</style>
