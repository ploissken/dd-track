<template>
  <v-sheet class="bg-transparent d-flex ga-2">
    <v-chip
      @click="$emit('changeChallenge', 'ALL_CHALLENGES')"
      :color="getColor('ALL_CHALLENGES')"
      :variant="getVariant('ALL_CHALLENGES')"
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
          :variant="getVariant(challenge.id)"
          :color="getColor(challenge.id)"
        >
          {{ `${challenge.emoji}` }}
        </v-chip>
      </template>
    </v-tooltip>
    <v-tooltip text="Tarefas Pessoais" location="top">
      <template v-slot:activator="{ props }">
        <v-chip
          v-bind="props"
          @click="$emit('changeChallenge', 'PERSONAL_CHALLENGES')"
          :color="getColor('PERSONAL_CHALLENGES')"
          :variant="getVariant('PERSONAL_CHALLENGES')"
        >
          🫵
        </v-chip>
      </template>
    </v-tooltip>
  </v-sheet>
</template>

<script setup lang="ts">
import challenges from "../../assets/challenges.json";

const emit = defineEmits(["changeChallenge"]);
const props = defineProps({
  currentChallengeFilter: String,
});

const getVariant = (type: string) => {
  return props.currentChallengeFilter === type ? "flat" : undefined;
};

const getColor = (type: string) => {
  return props.currentChallengeFilter === type ? "purple-darken-4" : "";
};
</script>
