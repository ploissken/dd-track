<template>
  <v-container
    class="pa-0 ma-0 d-flex flex-wrap justify-space-between align-start"
  >
    <v-row justify="start" class="m-2">
      <v-col>
        <label>locations</label>
        <MultiselectChips
          :options="challengeOptions"
          :selectedIds="currentChallengeFilter"
          @updated="(challengeId) => $emit('change:challenge', challengeId)"
        />
      </v-col>
      <v-col>
        <label>users</label>
        <MultiselectChips
          :options="users"
          :selectedIds="currentPersonFilter"
          @updated="(updatedArray) => $emit('change:person', updatedArray)"
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
import MultiselectChips from "./MultiselectChips.vue";
import users from "../../assets/users.json";
import challenges from "../../assets/challenges.json";

defineProps<{
  challenges: Array<Challenge>;
  currentPersonFilter: Array<String>;
  currentChallengeFilter: Array<String>;
  viewType: String;
}>();

const challengeOptions = [
  ...challenges.map(({ id, emoji }) => {
    return { id, label: emoji };
  }),
  { id: null, label: "🫵" },
];

const taskTitleFilter = ref("");
</script>
