<template>
  <v-sheet class="bg-transparent d-flex ga-2">
    <v-chip
      v-if="allowDisplayAll"
      @click="$emit('changeChallenge', 'ALL_CHALLENGES')"
      :color="
        currentChallengeFilter === 'ALL_CHALLENGES' ? 'purple-darken-4' : ''
      "
      :variant="
        currentChallengeFilter === 'ALL_CHALLENGES' ? 'flat' : undefined
      "
    >
      Tudo
    </v-chip>
    <v-tooltip
      v-for="challenge in challenges"
      :text="challenge.name"
      location="top"
    >
      <template v-slot:activator="{ props }">
        <v-chip
          v-bind="props"
          @click="$emit('changeChallenge', challenge.id)"
          :variant="
            currentChallengeFilter === challenge.id ? 'flat' : undefined
          "
          :color="
            currentChallengeFilter === challenge.id
              ? 'purple-darken-4'
              : undefined
          "
        >
          {{ `${challenge.emoji}` }}
        </v-chip>
      </template>
    </v-tooltip>
    <v-tooltip
      v-if="allowFilterByPersonal"
      text="Tarefas Pessoais"
      location="top"
    >
      <template v-slot:activator="{ props }">
        <v-chip
          v-bind="props"
          @click="$emit('changeChallenge', 'PERSONAL_CHALLENGES')"
          :color="
            currentChallengeFilter === 'PERSONAL_CHALLENGES'
              ? 'purple-darken-4'
              : ''
          "
          :variant="
            currentChallengeFilter === 'PERSONAL_CHALLENGES'
              ? 'flat'
              : undefined
          "
        >
          🫵
        </v-chip>
      </template>
    </v-tooltip>
  </v-sheet>
</template>

<script setup lang="ts">
import { onMounted } from "vue";
import challenges from "../../assets/challenges.json";

const emit = defineEmits(["changeChallenge"]);
const props = defineProps({
  allowDisplayAll: Boolean,
  allowFilterByPersonal: Boolean,
  currentChallengeFilter: String,
});

onMounted(() => {
  if (!props.allowDisplayAll) {
    emit("changeChallenge", challenges[0].id);
  }
});
</script>
