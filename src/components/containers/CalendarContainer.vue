<template>
  <v-container class="fill-height p-0 m-0 d-flex align-start justify-start">
    <v-row class="justify-center">
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
      </v-col>
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
import axios from "axios";
import { computed, onMounted, ref } from "vue";
import challenges from "../../assets/challenges.json";
import users from "../../assets/users.json";

const BOD_ID = users[2].id;
const initialDate = ref([new Date()]);
const currentChallengeFilter = ref("");
const currentTaskFilter = ref("");
const currentPersonFilter = ref(BOD_ID);
const type = ref("month");
const weekday = ref([0, 1, 2, 3, 4, 5, 6]);
const rawTaskData = ref([]);

const filteredEvents = computed(() => {
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

  return textFilteredTasks
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

const filterByTask = (taskName) => {
  console.log("look the event", taskName);
};

onMounted(() => {
  axios.get("https://habitica.txto.com.br/get-tasks").then((response) => {
    const tasks = response.data.data;
    rawTaskData.value = tasks;
  });
});
</script>
