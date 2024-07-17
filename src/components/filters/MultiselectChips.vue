<template>
  <v-sheet class="bg-transparent d-flex ga-2">
    <v-chip
      v-for="option in options"
      @click="() => updateSelected(option.id)"
      :color="selectedIds.includes(option.id) ? 'purple-darken-4' : undefined"
      :variant="selectedIds.includes(option.id) ? 'flat' : undefined"
    >
      {{ option.label }}
    </v-chip>
  </v-sheet>
</template>

<script setup lang="ts">
import { MultiselectOption } from "../../interfaces/MultiselectOption";

const emit = defineEmits(["updated"]);

const props = defineProps<{
  selectedIds: Array<String>;
  options: Array<MultiselectOption>;
}>();

const updateSelected = (clickedId: string) => {
  const updatedIdArray = props.selectedIds.includes(clickedId)
    ? props.selectedIds.filter((id) => id !== clickedId)
    : [...props.selectedIds, clickedId];

  emit("updated", updatedIdArray);
};
</script>
