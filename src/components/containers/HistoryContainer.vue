<template>
  <v-container class="fill-height p-0 m-0 d-flex align-start justify-start">
    <v-row class="justify-center">
      <v-col cols="12" lg="8" class="d-flex justify-center">
        <h2 class="ma-2">Histórico</h2>
      </v-col>
      <v-col cols="12" lg="8">
        <HistoryFilters
          @changePerson="(id: string) => (currentPersonFilter = id)"
          @changeChallenge="(id: string) => (currentChallengeFilter = id)"
          :challenges="challenges"
          :current-person-filter="currentPersonFilter"
          :current-challenge-filter="currentChallengeFilter"
        />
      </v-col>
      <v-col
        v-if="!rawTaskData.length"
        cols="12"
        class="d-flex align-center justify-center"
      >
        <v-progress-circular indeterminate />
      </v-col>
      <v-col v-else cols="12" lg="8">
        <HistoryTable
          :challenges="challenges"
          :filtered-tasks="filteredTasks"
        />
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup lang="ts">
import { ref, onMounted, computed } from "vue";
import axios from "axios";
import challenges from "../../assets/challenges.json";
import dayjs from "dayjs";
import ptBr from "dayjs/locale/pt-br";
dayjs.locale(ptBr);

const rawTaskData = ref([]);
const currentChallengeFilter = ref("");
const currentPersonFilter = ref("");
const taskData = ref(null);

const filteredTasks = computed(() => {
  let personFilteredTasks =
    currentPersonFilter.value === ""
      ? rawTaskData.value
      : rawTaskData.value.filter(
          ({ user_id }) => user_id === currentPersonFilter.value,
        );

  return currentChallengeFilter.value === ""
    ? personFilteredTasks
    : personFilteredTasks.filter(
        ({ challenge_id }) => challenge_id === currentChallengeFilter.value,
      );
});

onMounted(() => {
  axios.get("https://habitica.txto.com.br/get-tasks").then((response) => {
    rawTaskData.value = response.data.data;
    taskData.value = response.data.data;
  });
});
</script>
