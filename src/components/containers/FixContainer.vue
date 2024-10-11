<template>
  <v-container fluid>
    <v-row class="justify-center d-flex">
      <v-col cols="12" class="d-flex justify-center">
        <h2 class="ma-2">Fix & Tweaks</h2>
      </v-col>
      <v-col cols="3">
        <v-row>
          <v-col cols="12" class="d-flex justify-center">
            <v-chip
              v-for="user in users"
              class="mr-2"
              @click="currentPersonFilter = user.id"
              :variant="currentPersonFilter === user.id ? 'flat' : undefined"
            >
              {{ user.label }}
            </v-chip>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="12">
            <v-text-field v-model="taskId" label="task id" />
          </v-col>
          <v-col cols="12">
            <v-btn block @click="getUserProfile"> get user profile </v-btn>
          </v-col>
          <v-col cols="12">
            <v-btn block @click="getUserTasks"> get user tasks </v-btn>
          </v-col>
          <v-col cols="12">
            <v-btn block @click="postTaskScore"> post task score </v-btn>
          </v-col>
          <v-col cols="12">
            <v-btn block disabled> accept challenge </v-btn>
          </v-col>
          <v-col cols="12">
            <v-btn block disabled> run cron </v-btn>
          </v-col>
          <v-col cols="12">
            <v-btn block @click="deleteTask"> delete task </v-btn>
          </v-col>
        </v-row>
      </v-col>
      <v-col cols="4">
        <v-textarea
          v-model="response"
          label="response"
          :loading="loading"
          rows="25"
        />
      </v-col>
      <v-col cols="7">
        <CreateTaskContainer />
      </v-col>
    </v-row>

    <v-dialog v-model="dialogOpen" max-width="400" persistent>
      <v-card
        prepend-icon="mdi-lock-alert"
        text="Qual a palavra mágica pra entrar aqui?"
        title="Pera lá, forasteire"
      >
        <v-row dense class="d-flex justify-center">
          <v-col cols="11">
            <v-text-field
              ref="pwdField"
              v-model="passwordValue"
              label="palavra mágica"
              :error-messages="validationCount && `será???`"
              required
              autofocus
            />
          </v-col>
        </v-row>
        <template v-slot:actions>
          <v-spacer />
          <v-btn @click="validatePassword" color="primary"> Taí pai </v-btn>
        </template>
      </v-card>
    </v-dialog>
  </v-container>
</template>

<script lang="ts" setup>
import { ref } from "vue";
import { useRouter } from "vue-router";
const dialogOpen = ref(true);
const loading = ref(false);
const passwordValue = ref("");
const taskId = ref("");
const response = ref("");
const currentPersonFilter = ref("a5a7c27c-1dfc-4155-804c-e458d24cdba2");
const pwdField = ref<HTMLInputElement>();
const validationCount = ref(0);
import axios from "axios";
import CreateTaskContainer from "./CreateTaskContainer.vue";
const router = useRouter();
const apiURL = "https://habitica.txto.com.br";
const users = [
  { label: "👩🏽‍🌾", id: "a5a7c27c-1dfc-4155-804c-e458d24cdba2" },
  { label: "👨🏼‍💻", id: "9ae7f3de-9061-4f4b-829c-586dd7dcc12a" },
];

const getUserProfile = () => {
  loading.value = true;
  response.value = "";
  axios
    .get(`${apiURL}/get-user-profile?id=${currentPersonFilter.value}`)
    .then((res) => {
      response.value = JSON.stringify(res.data, 3, 3);
      loading.value = false;
    });
};

const getUserTasks = () => {
  loading.value = true;
  response.value = "";
  axios
    .get(`${apiURL}/get-user-tasks?id=${currentPersonFilter.value}`)
    .then((res) => {
      response.value = JSON.stringify(res.data, 3, 3);
      loading.value = false;
    });
};

const postTaskScore = () => {
  if (!taskId.value) return;
  loading.value = true;
  response.value = "";
  axios
    .post(`${apiURL}/post-task-score?id=${currentPersonFilter.value}`, {
      taskId: taskId.value,
    })
    .then((res) => {
      response.value = JSON.stringify(res.data, 3, 3);
      loading.value = false;
    });
};

const deleteTask = () => {
  if (!taskId.value) return;
  loading.value = true;
  response.value = "";
  axios
    .post(`${apiURL}/delete-task?id=${currentPersonFilter.value}`, {
      taskId: taskId.value,
    })
    .then((res) => {
      response.value = JSON.stringify(res.data, 3, 3);
      loading.value = false;
    });
};

const validatePassword = () => {
  // TODO: check password in the backend

  validationCount.value = validationCount.value + 1;

  if (validationCount.value > 2) {
    router.push("/");
  }
};
</script>
