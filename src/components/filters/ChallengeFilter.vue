<template>
  <v-sheet class="bg-transparent d-flex ga-2">
    <v-chip
      @click="$emit('changeChallenge', CHALLENGE.all)"
      :color="getColor(CHALLENGE.all)"
      :variant="getVariant(CHALLENGE.all)"
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
          @click="$emit('changeChallenge', CHALLENGE.personal)"
          :color="getColor(CHALLENGE.personal)"
          :variant="getVariant(CHALLENGE.personal)"
        >
          🫵
        </v-chip>
      </template>
    </v-tooltip>
  </v-sheet>
</template>

<script setup lang="ts">
import challenges from "../../assets/challenges.json";
import { CHALLENGE } from "../../interfaces/Constants";

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
