<template>
  <div :class="'task' + (done ? ' done' : '')">
    <input
      class="checkbox"
      type="checkbox"
      v-model="checked"
      @change="$emit('update', checked)"
    />

    <div class="title" v-if="!beingEdited" @click="$emit('makeEditable')">
      {{ title }}
    </div>

    <input
      type="text"
      v-model="newItemTitle"
      v-if="beingEdited"
      v-on:keyup.enter="$emit('update', newItemTitle)"
    />
    <button
      type="button"
      class="save"
      v-if="beingEdited"
      @click="$emit('update', newItemTitle)"
    >
      s
    </button>

    <button @click="$emit('remove')" class="remove">X</button>
  </div>
</template>

<script setup lang="ts">
import { defineProps, defineEmits, ref } from "vue";
const props = defineProps({
  title: String,
  done: Boolean,
  beingEdited: Boolean,
});
const emits = defineEmits<{
  (event: "remove"): void;
  (event: "makeEditable"): void;
  (event: "update", newItemTitle: string | boolean): void;
}>();
const newItemTitle = ref(props.title);
const checked = ref(props.done);
</script>

<style>
.task {
  margin: 5px 0;
}

.title {
  transition: ease-in-out 0.1s;
  margin: 0 10px;
  padding: 2px 10px;
}

div.title {
  font-size: 1em;
  font-weight: 500;
  width: 243px;
  padding: 0px 10px;
  display: inline-block;
}

.remove {
  background-color: #202020;
  width: auto;
  font-size: small;
  padding-left: 12px;
  padding-right: 12px;
}

.remove:hover {
  background-color: #a60000;
  border-color: #a60000;
}
.done div {
  text-decoration: line-through;
  color: #a6a6a6;
}

.done button {
  background-color: #450000;
}
button.save {
  background-color: #202020;
  width: auto;
  font-size: small;
  padding-left: 12px;
  padding-right: 12px;
}
</style>
