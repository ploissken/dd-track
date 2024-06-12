<template>
  <v-container class="fill-height p-0 m-0 d-flex align-start">
    <v-row justify="start" class="m-2">
      <v-col>
        <ChallengeFilter
          @changeChallenge="
            (challengeId) => $emit('changeChallenge', challengeId)
          "
          :currentChallengeFilter="currentChallengeFilter"
          :allowFilterByPersonal="true"
        />
      </v-col>
      <v-col class="d-flex ga-2">
        <UserFilter
          @changePerson="(userId) => $emit('changePerson', userId)"
          :currentPersonFilter="currentPersonFilter"
        />
      </v-col>
      <v-col>
        <v-spacer />
      </v-col>
      <v-col>
        <v-text-field
          v-model="taskTitleFilter"
          @update:modelValue="(newValue) => $emit('filter-by:task', newValue)"
          label="Filtrar tarefas por"
          variant="outlined"
        />
        <!-- there is a bug in timestamp parsing over week-view, (v-calendar is still experimental) -->
        <!-- <v-select
          v-model="type"
          :items="types"
          class="ma-2"
          label="View Mode"
          variant="outlined"
          dense
          hide-details
          @update:modelValue="(viewStyle) => $emit('changeView', viewStyle)"
        /> -->
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup lang="ts">
import { ref } from "vue";

export interface Challenge {
  id: string;
  name: string;
  emoji: string;
}

const props = defineProps({
  challenges: Array<Challenge>,
  currentPersonFilter: String,
  currentChallengeFilter: String,
});

const type = ref("month");
const types = ref(["month", "week", "day"]);
const taskTitleFilter = ref("");
</script>
