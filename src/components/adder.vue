<template>
  <div class="adder">
    <input type="text" v-model="newItemTitle"/>
    <button type="button" @click="$emit('add', newItemTitle); newItemTitle=''">add</button>
  </div>
</template>

<script setup lang="ts">
import { defineEmits, ref } from "vue";
const emits = defineEmits<{
  (event: "add", arg: string): void;
}>();
const newItemTitle = ref("");
</script>

<style>
.adder{
  margin: 50px 60px;
}
</style>
