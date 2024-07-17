<template>
  <v-container class="fill-height d-flex align-start justify-start">
    <v-row class="justify-center">
      <v-col cols="12" lg="4">
        <v-row>
          <v-col cols="6" lg="12">
            <label> tasks by user </label>
            <DoughnutChart
              class="mb-6"
              :style="{
                height: '200px',
              }"
              :chartData="userDistribuitonData"
            />
          </v-col>
          <v-col cols="6" lg="12">
            <label class="mt-6">tasks by place </label>
            <BarChart
              :style="{
                height: '200px',
              }"
              :chartData="locationDistribuitonData"
              :options="{ plugins: { legend: { display: false } } }"
            />
          </v-col>
        </v-row>
      </v-col>
      <v-col cols="12" lg="8">
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
import users from "../../assets/users.json";
import dayjs from "dayjs";
import ptBr from "dayjs/locale/pt-br";
import { DoughnutChart, BarChart } from "vue-chart-3";
import { Chart, registerables } from "chart.js";
import { Task } from "../../interfaces/Task";
import { computed } from "vue";
dayjs.locale(ptBr);
Chart.register(...registerables);

const props = defineProps({
  loading: Boolean,
  currentPersonFilter: String,
  filteredTasks: Array<Task>,
  currentTaskFilter: {
    type: String,
    default: "",
  },
  currentChallengeFilter: String,
});

const userDistribuitonData = computed(() => {
  return {
    labels: users.map(({ label }) => label),
    datasets: [
      {
        data: users.map(
          ({ id }) =>
            props.filteredTasks?.filter((task) => task.user_id === id).length,
        ),
        backgroundColor: ["green", "purple"],
        borderColor: ["green", "purple"],
      },
    ],
  };
});

const locationDistribuitonData = computed(() => {
  return {
    labels: challenges.map(({ emoji }) => emoji),
    datasets: [
      {
        data: challenges.map(
          ({ id }) =>
            props.filteredTasks?.filter((task) => task.challenge_id === id)
              .length,
        ),
        backgroundColor: ["purple", "green", "orange", "teal", "gray"],
      },
    ],
  };
});
</script>
