<template>
  <div :class="'task' + (done ? ' done' : '')">
    <input
      class="checkbox"
      type="checkbox"
      :checked="done ? true : false"
      @change="$emit('check', $event)"
    />

    <div class="title" v-if="!beingEdited">{{ title }}</div>
    <button @click="$emit('edit')" v-if="!beingEdited">edit</button>

    <form @submit.prevent="$emit('save', $event)" class="add" v-if="beingEdited">
      <input class="title" :value="title" type="text" />
      <button type="submit">save</button>
    </form>

    <button @click="$emit('remove')" class="remove">X</button>
  </div>
</template>

<!--<script lang="ts">
export default {
  props: ["title", "done", "beingEdited", "save"],
  emits: ["remove", "edit", "save", "check"],
  data() {
    return {};
  },
  // methods: {
  //   edit(state: boolean) {
  //     this.beingEdited = state;
  //   }
  // }
};
</script> -->

<script setup lang="ts">
const props = defineProps({
  title: String,
  done: Boolean,
  beingEdited: Boolean
})
const emits = defineEmits<{
  (event: 'remove'): void
  (event: 'edit'): void
  (event: 'save', eventArg: Event): void
  (event: 'check', eventArg: Event): void

}>()

</script>

<style>
/* .task{
  display: inline-block;
} */
</style>
