<template>
  <v-container class="fill-height pa-0 ma-0 d-flex align-start justify-start">
    <v-row class="justify-center">
      <v-col cols="12">
        <HistoryTable
          :challenges="challenges"
          :filtered-tasks="filteredTasks"
        />
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup lang="ts">
import challenges from "../../assets/challenges.json";
import dayjs from "dayjs";
import ptBr from "dayjs/locale/pt-br";
import { Task } from "../../interfaces/Task";
dayjs.locale(ptBr);

defineProps({
  loading: Boolean,
  currentPersonFilter: String,
  filteredTasks: Array<Task>,
  currentTaskFilter: {
    type: String,
    default: "",
  },
  currentChallengeFilter: String,
});
</script>
