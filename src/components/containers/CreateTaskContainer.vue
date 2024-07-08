<template>
  <v-container class="fill-height p-0 m-0 d-flex align-start">
    <v-row class="justify-center">
      <v-col cols="12" lg="7">
        <h2 class="ma-2 d-flex justify-center">Criar Tarefa</h2>
      </v-col>
      <v-col cols="12" lg="7">
        <v-form
          ref="form"
          v-model="valid"
          :disabled="loading"
          @submit.prevent="handleOnSubmit"
        >
          <v-card color="surface-variant" rounded="lg" variant="outlined">
            <v-row class="mt-2 d-flex justify-center ga-2 pa-2">
              <v-col cols="12">
                <v-text-field
                  v-model="taskTitle"
                  label="Tarefa"
                  :rules="[
                    (value) => !!value || 'Esse campo é obrigatório, oras.',
                  ]"
                />
              </v-col>
              <v-col cols="12">
                <p>Local</p>
                <v-chip-group
                  v-model="taskPlace"
                  selected-class="text-deep-purple"
                  column
                >
                  <v-chip
                    v-for="challenge in challenges"
                    :key="challenge.id"
                    :value="challenge.id"
                    :text="`${challenge.emoji} ${challenge.name}`"
                  />
                </v-chip-group>
              </v-col>
              <v-col cols="12">
                <p>Tipo</p>
                <v-btn-toggle
                  v-model="taskType"
                  color="deep-purple"
                  rounded="0"
                  group
                >
                  <v-btn value="habit"> Habit </v-btn>
                  <v-btn value="daily"> Daily </v-btn>
                  <v-btn value="todo"> Todo </v-btn>
                </v-btn-toggle>
              </v-col>
              <v-col cols="12">
                <v-textarea v-model="taskNotes" label="Observações" />
              </v-col>
              <v-col cols="12">
                <p>Dificuldade</p>
                <v-btn-toggle
                  v-model="taskLevel"
                  color="deep-purple"
                  rounded="0"
                  group
                >
                  <v-btn value="0.1"> 😁 </v-btn>
                  <v-btn value="1"> 🙂 </v-btn>
                  <v-btn value="1.5"> 😪 </v-btn>
                  <v-btn value="2"> 😮‍💨 </v-btn>
                </v-btn-toggle>
              </v-col>
              <v-col cols="12" class="d-flex justify-end">
                <v-btn
                  color="deep-purple-accent-3"
                  :loading="loading"
                  type="submit"
                >
                  Salvar
                </v-btn>
              </v-col>
            </v-row>
          </v-card>
        </v-form>
      </v-col>
    </v-row>
    <v-snackbar
      v-model="toastOpen"
      timeout="3000"
      location="top"
      color="deep-purple-accent-4"
    >
      ✅ Task criada!
    </v-snackbar>
  </v-container>
</template>

<script lang="ts" setup>
import { ref, reactive } from "vue";
import challenges from "../../assets/challenges.json";
import axios from "axios";

const valid = ref(false);
const loading = ref(false);
const toastOpen = ref(false);
const form = ref<HTMLFormElement>();
const taskTitle = ref("");
const taskNotes = ref("");
const taskType = ref("todo");
const taskLevel = ref("1");
const taskPlace = ref(challenges[4].id);

const handleOnSubmit = async () => {
  if (!valid.value) {
    return;
  }
  const newTask = {
    task: {
      text: taskTitle.value,
      notes: taskNotes.value,
      type: taskType.value,
      priority: taskLevel.value,
    },
    challengeId: taskPlace.value,
  };
  loading.value = true;

  axios
    // .post("http://localhost:9000/create-task", newTask)
    .post("https://habitica.txto.com.br/create-task", newTask)
    .then(() => {
      toastOpen.value = true;
      clearForm();
    });
};

const clearForm = () => {
  loading.value = false;
  taskTitle.value = "";
  taskNotes.value = "";
  taskPlace.value = challenges[4].id;
  taskType.value = "todo";
  taskLevel.value = "1";
  form.value?.reset();
  form.value?.resetValidation();
};
</script>
