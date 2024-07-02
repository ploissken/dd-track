<template>
  <v-container class="fill-height p-0 m-0 d-flex align-start justify-start">
    <v-col cols="12">
      <CalendarFilters
        @changeView="(view: string) => (type = view)"
        @changePerson="(id: string) => (currentPersonFilter = id)"
        @changeChallenge="(id: string) => (currentChallengeFilter = id)"
        @filter-by:task="(text: string) => (currentTaskFilter = text)"
        :challenges="challenges"
        :current-person-filter="currentPersonFilter"
        :current-challenge-filter="currentChallengeFilter"
      />
      <v-btn-toggle v-model="taskLevel" color="deep-purple" rounded="0" group>
        <v-btn value="calendar"> 📆 </v-btn>

        <v-btn value="table"> 📜 </v-btn>
      </v-btn-toggle>
    </v-col>
    <v-row class="justify-center">
      <v-col
        v-if="loading"
        cols="12"
        class="d-flex align-center justify-center"
      >
        <v-progress-circular indeterminate />
      </v-col>
      <v-col v-else cols="12">
        <HistoryContainer
          v-if="taskLevel === 'table'"
          :filteredEvents="filteredEvents"
        />
        <CalendarContainer
          v-if="taskLevel === 'calendar'"
          :filteredEvents="calendarEvents"
        />
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup lang="ts">
import axios from "axios";
import { computed, onMounted, ref } from "vue";
import challenges from "../../assets/challenges.json";
import users from "../../assets/users.json";
import CalendarContainer from "./CalendarContainer.vue";
import HistoryContainer from "./HistoryContainer.vue";

const BOD_ID = users[2].id;
const loading = ref(true);
const currentChallengeFilter = ref(challenges[1].id);
const currentTaskFilter = ref("");
const currentPersonFilter = ref(BOD_ID);
const rawTaskData = ref([]);
const taskLevel = ref("table");

export interface CalendarEvent {
  title: String;
  start: Date;
  end: Date;
  color: String;
}

const filteredEvents = computed((): any[] => {
  const personFilteredTasks =
    currentPersonFilter.value === ""
      ? rawTaskData.value
      : rawTaskData.value.filter(
          ({ user_id }) => user_id === currentPersonFilter.value,
        );

  const challengeFilteredTasks = personFilteredTasks.filter(
    ({ challenge_id }) => challenge_id === currentChallengeFilter.value,
  );

  const textFilteredTasks = challengeFilteredTasks.filter(({ task_text }) =>
    (task_text as string)
      .toLowerCase()
      .includes(currentTaskFilter.value.toLowerCase()),
  );

  return textFilteredTasks;
});

const calendarEvents = computed((): CalendarEvent[] => {
  return filteredEvents.value
    .map(({ task_text, created_at, user_id }) => {
      return {
        title: task_text,
        start: new Date(created_at),
        end: new Date(created_at),
        color: user_id === BOD_ID ? "deep-purple" : "green",
      };
    })
    .reverse();
});

onMounted(() => {
  loading.value = true;
  axios.get("https://habitica.txto.com.br/get-tasks").then((response) => {
    const tasks = response.data.data;
    rawTaskData.value = tasks;
    loading.value = false;
  });
});
</script>
