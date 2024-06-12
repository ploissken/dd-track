<template>
  <v-container class="fill-height p-0 m-0 d-flex align-start">
    <v-row class="mt-2">
      <v-col cols="12">
        <UserFilter
          @changePerson="(userId) => $emit('changePerson', userId)"
          :currentPersonFilter="currentPersonFilter"
        />
      </v-col>
      <v-col class="d-flex ga-2" cols="12">
        <ChallengeFilter
          allowDisplayAll
          @changeChallenge="
            (challengeId) => $emit('changeChallenge', challengeId)
          "
          :currentChallengeFilter="currentChallengeFilter"
        />
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup lang="ts">
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
</script>
