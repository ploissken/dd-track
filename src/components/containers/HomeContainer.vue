<template>
  <v-container class="fill-height d-flex align-start justify-start">
    <v-col cols="12" class="ma-0 pa-0">
      <HomeFilters
        @change:view="(view: string) => (viewType = view)"
        @changePerson="(id: [string]) => (currentPersonFilter = id)"
        @changeChallenge="(id: [string]) => (currentChallengeFilter = id)"
        @filter-by:task="(text: string) => (currentTaskFilter = text)"
        :challenges="challenges"
        :current-person-filter="currentPersonFilter"
        :current-challenge-filter="currentChallengeFilter"
        :view-type="viewType"
      />
    </v-col>
    <v-row class="justify-center">
      <v-col
        v-if="loading"
        cols="12"
        class="d-flex align-center justify-center"
      >
        <v-progress-circular indeterminate />
      </v-col>
      <v-col v-else cols="12" class="ma-0 pa-0">
        <HistoryContainer
          v-if="viewType === 'table'"
          :filteredTasks="filteredTasks"
        />
        <CalendarContainer
          v-if="viewType === 'calendar'"
          :calendarEvents="calendarEvents"
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
import { CalendarEvent } from "../../interfaces/CalendarEvent";
import { Task } from "../../interfaces/Task";
import { CHALLENGE } from "../../interfaces/Constants";

const BOD_ID = users[1].id;
const loading = ref(true);
const currentChallengeFilter = ref([challenges[0].id, challenges[1].id]);
const currentTaskFilter = ref("");
const currentPersonFilter = ref([BOD_ID]);
const rawTaskData = ref([]);
const viewType = ref("calendar");

const filteredTasks = computed((): Task[] => {
  const personFilteredTasks = rawTaskData.value.filter(({ user_id }) =>
    currentPersonFilter.value.includes(user_id),
  );

  const challengeFilteredTasks = personFilteredTasks.filter(
    ({ challenge_id }) => currentChallengeFilter.value.includes(challenge_id),
  );

  const textFilteredTasks = challengeFilteredTasks.filter(({ task_text }) =>
    (task_text as string)
      .toLowerCase()
      .includes(currentTaskFilter.value.toLowerCase()),
  );

  return textFilteredTasks;
});

const calendarEvents = computed((): CalendarEvent[] => {
  return filteredTasks.value
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
