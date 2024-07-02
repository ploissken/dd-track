<template>
  <v-container class="fill-height p-0 m-0 d-flex align-start justify-start">
    <v-row class="justify-center">
      <v-col cols="12" lg="8">
        <HistoryTable
          :challenges="challenges"
          :filtered-tasks="filteredEvents"
        />
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup lang="ts">
import challenges from "../../assets/challenges.json";
import dayjs from "dayjs";
import ptBr from "dayjs/locale/pt-br";
dayjs.locale(ptBr);

defineProps({
  loading: Boolean,
  currentPersonFilter: String,
  filteredEvents: Array,
  currentTaskFilter: {
    type: String,
    default: "",
  },
  currentChallengeFilter: String,
});
</script>
