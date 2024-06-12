<template>
  <v-container class="fill-height p-0 m-0 d-flex align-start">
    <v-row>
      <v-col cols="12">
        <v-card color="surface-variant" rounded="lg" variant="outlined">
          <v-table fixed-header>
            <thead>
              <tr>
                <th></th>
                <th>📍</th>
                <th>Tarefa</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="task in filteredTasks">
                <td>
                  {{ getUserName(task.user_id) }}
                </td>
                <td>
                  {{ getChallengeSymbol(task.challenge_id) }}
                </td>
                <td>
                  <p>
                    {{ task.task_text }}
                  </p>
                  <p class="font-weight-thin font-italic">
                    {{ dayjs(task.created_at).format("D/MM (ddd) HH:mm") }}
                  </p>
                </td>
              </tr>
            </tbody>
          </v-table>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup lang="ts">
import dayjs from "dayjs";
import ptBr from "dayjs/locale/pt-br";
import { Challenge } from "./HistoryFilters.vue";
dayjs.locale(ptBr);

interface Task {
  user_id: string;
  task_text: string;
  challenge_id: string;
  created_at: string;
}

const props = defineProps<{
  filteredTasks: Array<Task>;
  challenges: Array<Challenge>;
}>();

const getUserName = (id: string) => {
  return id === "a5a7c27c-1dfc-4155-804c-e458d24cdba2" ? "👩🏽‍🌾" : "👨🏼‍💻";
};

const getChallengeSymbol = (id: string) => {
  const challenge = props.challenges.find((challenge) => challenge.id === id);
  return challenge?.id ? `${challenge?.emoji}` : "";
};
</script>
