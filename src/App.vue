<template>
  <ul>
    <task v-for="task in tasks" :title="task.title" :done="task.done" :index="task.index"
      :being-edited="task.beingEdited" 
      
      @remove="remove(task)"
      @edit="edit(task)" 
      @check="check(task, $event)"
      @save="save(task, $event)"/>


  </ul>
  <form @submit.prevent="add" class="add">
    <input type="text"/>    
    <button type="submit">add</button>
  </form>
  
  <!-- <button @click="add">add</button> -->
</template>

<!-- <script setup lang="ts">
import task from './components/task.vue';

var task1 = 
  {
    title: "2123121231231",
    done: true,
    element: "span"
  };

var tasks = [task1];
///re
function add() {
  console.log("add");
  task1.title = "12";
  tasks.push({
    title: "2123121231231",
    done: true,
    element: "span"
  });
}
function remove() {
  console.log("remove");
}


</script> -->



<script lang="ts">
import task from './components/task.vue';

export default {
  components: { task },
  data() {
    return {
      tasks: [] as Array<{ title: string, done: boolean, index: number, beingEdited: boolean }>,
      lastIndex: 0 as Number
    }
  },
  beforeMount() {
    this.tasks.push({
      title: "2123121231231",
      done: true,
      index: 0,
      beingEdited: false
    })
    this.lastIndex = this.tasks[0].index + 1;

  },
  methods: {
    add(event: SubmitEvent) {
      if(event.target[0].value === "") return;
      this.tasks.push({
        // title: this.tasks.length.toString(),
        title: event.target[0].value,
        done: false,
        index: this.lastIndex++,
        beingEdited: false
      });
      event.target.reset();
    },
    edit(task: { title: string, done: boolean, index: number, beingEdited: boolean }) {
      this.tasks.forEach((t) => {
        if (t.index !== task.index) {
          t.beingEdited = false;
        }
      });
      task.beingEdited = true;
    },
    remove(task: { title: string, done: boolean, index: number, beingEdited: boolean }) {
      this.tasks = this.tasks.filter((t) => t.index !== task.index);
    },
    check(task: { title: string, done: boolean, index: number, beingEdited: boolean }, event: Event) {
      task.done = (event.target as HTMLInputElement).checked;
    },
    save(task: { title: string, done: boolean, index: number, beingEdited: boolean }, event: SubmitEvent) {
      if(event.target[0].value !== "") task.title = event.target[0].value;
      
      task.beingEdited = false;
    }


  }
}

</script>
  
<style>

</style>
