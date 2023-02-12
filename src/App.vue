<template>
  <ul>
    <task
      v-for="task in tasks"
      :title="task.title"
      :done="task.isDone"
      :being-edited="currentEdit === task.id"
      @remove="remove(task)"
      @makeEditable="makeEditable(task.id)"
      @update="update(task, $event)"
    />
  </ul>
  <adder @add="add" />

</template>

<script setup lang="ts">
import task from "./components/task.vue";
import adder from "./components/adder.vue";
import axios from "axios";
import { ref, onMounted } from "vue";

interface Task {
  title: string;
  isDone: boolean;
  id: number;
  // beingEdited: boolean;
}

const api = axios.create({ baseURL: "http://127.0.0.1:8000/items" });

const tasks = ref([] as Array<Task>);

const currentEdit = ref(null as number | null);

async function sync() {
  let resp = await api.get("");
  console.log(resp.data);
  tasks.value = resp.data;
}

async function add(arg: string) {
  let resp = await api.post("create", { title: arg });
  tasks.value.push(resp.data);
}

function makeEditable(id: number) {
  currentEdit.value = id;
}

function remove(task: Task) {
  api.post("delete", { id: task.id });
  tasks.value = tasks.value.filter((t) => t.id !== task.id);
}

async function update(task: Task, event: string | boolean) {
  let payload = { id: task.id, title: task.title, isDone: task.isDone };
  typeof event === "string" ? (payload.title = event) : (payload.isDone = event);
  let resp = await api.post("update", payload);
  currentEdit.value = null;

  task.title = resp.data.title;
  task.isDone = resp.data.isDone;
}

onMounted(() => {
  sync();
});
</script>

<style></style>
