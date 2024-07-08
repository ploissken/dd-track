<template>
  <v-container class="fill-height d-flex align-start">
    <v-col cols="12" v-for="task in filteredTasks" class="bg-grey-darken-4">
      <v-row>
        <v-col cols="1">{{ getUserName(task.user_id) }}</v-col>
        <v-col cols="1">{{ getChallengeSymbol(task.challenge_id) }}</v-col>
        <v-col cols="10">
          <p>{{ task.task_text }}</p>
          <p class="font-weight-thin font-italic">
            {{ dayjs(task.created_at).format("D/MM (ddd) HH:mm") }}
          </p>
        </v-col>
      </v-row>
    </v-col>
  </v-container>
</template>

<script setup lang="ts">
import dayjs from "dayjs";
import ptBr from "dayjs/locale/pt-br";
import { Challenge } from "../interfaces/Challenge";
import { Task } from "../interfaces/Task";
dayjs.locale(ptBr);

const props = defineProps<{
  filteredTasks: Array<Task>;
  challenges: Array<Challenge>;
}>();

const getUserName = (id: string) => {
  return id === "a5a7c27c-1dfc-4155-804c-e458d24cdba2" ? "👩🏽‍🌾" : "👨🏼‍💻";
};

const getChallengeSymbol = (id: string) => {
  if (id === null) {
    return "🫵";
  }
  const challenge = props.challenges.find((challenge) => challenge.id === id);
  return challenge?.id ? `${challenge?.emoji}` : "";
};
</script>
