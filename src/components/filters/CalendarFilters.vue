<template>
  <v-container
    class="pa-0 ma-0 d-flex flex-wrap justify-space-between align-start"
  >
    <v-row justify="start" class="m-2">
      <v-col>
        <label>location</label>
        <ChallengeFilter
          @changeChallenge="
            (challengeId) => $emit('changeChallenge', challengeId)
          "
          :currentChallengeFilter="currentChallengeFilter"
          :allowFilterByPersonal="true"
          :allowDisplayAll="true"
        />
      </v-col>
      <v-col>
        <label>user</label>
        <UserFilter
          @changePerson="(userId) => $emit('changePerson', userId)"
          :currentPersonFilter="currentPersonFilter"
        />
      </v-col>
      <v-col>
        <label>view</label>
        <ViewFilters
          @change:view="(view) => $emit('change:view', view)"
          :view-type="viewType"
        />
      </v-col>
      <v-col cols="12" lg="4">
        <v-text-field
          v-model="taskTitleFilter"
          @update:modelValue="(newValue) => $emit('filter-by:task', newValue)"
          label="Filtrar tarefas por"
          variant="outlined"
        />
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup lang="ts">
import { ref } from "vue";
import { Challenge } from "../../interfaces/Challenge";

const props = defineProps({
  challenges: Array<Challenge>,
  currentPersonFilter: String,
  currentChallengeFilter: String,
  viewType: String,
});

const taskTitleFilter = ref("");
</script>
