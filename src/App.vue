<template>
  <ul>
    <task
      v-for="task in tasks"
      :title="task.title"
      :done="task.done"
      :_id="task._id"
      :being-edited="task.beingEdited"
      @remove="remove(task)"
      @edit="edit(task)"
      @check="check(task, $event)"
      @save="save(task, $event)"
    />
  </ul>
  <form @submit.prevent="add" class="add">
    <input type="text" />
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
import task from "./components/task.vue";
import axios from "axios";

export default {
  components: { task },
  data() {
    return {
      tasks: [] as Array<{
        title: string;
        done: boolean;
        _id: string;
        beingEdited: boolean;
      }>,
    };
  },
  beforeMount() {
    this.sync();
  },
  methods: {
    add(event: SubmitEvent) {
      if (event.target[0].value === "") return;
      axios
        .post("http://127.0.0.1:8000/items/create", {
          title: event.target[0].value,
        })
        .then((response) => {
          this.tasks.push(response.data);
        })
        .catch((error) => {
          console.log(error);
        });
      event.target.reset();
    },

    edit(task: { title: string; done: boolean; _id: string; beingEdited: boolean }) {
      this.tasks.forEach((t) => {
        if (t._id !== task._id) {
          t.beingEdited = false;
        }
      });
      task.beingEdited = true;
    },
    remove(task: { title: string; done: boolean; _id: string; beingEdited: boolean }) {
      axios
        .post("http://127.0.0.1:8000/items/delete", {
          _id: task._id,
        })
        .then((response) => {
          this.sync();
        })
        .catch((error) => {
          console.log(error);
        });
      this.sync();
    },
    check(
      task: { title: string; done: boolean; _id: string; beingEdited: boolean },
      event: Event
    ) {
      axios
        .post("http://127.0.0.1:8000/items/update", {
          _id: task._id,
          done: (event.target as HTMLInputElement).checked,
        })
        .then((response) => {
          this.sync();
        });
    },
    save(
      task: { title: string; done: boolean; _id: string; beingEdited: boolean },
      event: SubmitEvent
    ) {
      axios
        .post("http://127.0.0.1:8000/items/update", {
          _id: task._id,
          title: event.target[0].value,
        })
        .then((response) => {
          this.sync();
        });
    },
    sync() {
      axios
        .get("http://127.0.0.1:8000/items")
        .then((response) => {
          this.tasks = response.data;
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
};
</script>

<style></style>
