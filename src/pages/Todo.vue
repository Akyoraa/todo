<template>
  <q-page class="bg-grey-4 column">
    <div class="row q-pa-sm bg-primary">
      <q-input v-model="newTask" @keyup.enter="addTask" class="col" filled bg-color="white" placeholder="Add Task" dense>
        <template v-slot:append>
          <q-btn @click="addTask" round dense flat icon="add" />
        </template>
      </q-input>
    </div>
    <q-list class="bg-white" separator bordered>
      <q-item v-for="(task, index) in tasks" :key="index" @click="task.done = !task.done"
        :class="{ 'done bg-blue-1': task.done }" clickable v-ripple>
        <q-item-section avatar>
          <q-checkbox v-model="task.done" class="no-pointer-events" color="primary" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
          <q-btn @click.stop="deleteTask(index)" flat round dense color="primary" icon="delete" />
        </q-item-section>
      </q-item>
    </q-list>
    <div v-if="!tasks.length" class="no-tasks absolute-center">
      <q-icon name="check" size="100px" color="primary"></q-icon>
      <div class="text-h5 text-primary text-center">No Tasks</div>
    </div>
  </q-page>
</template>
<script setup>
import { defineComponent } from "vue";
import { ref } from "vue";
import { useQuasar } from "quasar";

const $q = useQuasar();
const newTask = ref("");
const tasks = ref([]);

defineComponent({
  name: "TodoPage",
});
function deleteTask(index) {
  $q.dialog({
    title: "Confirm",
    message: "Realy delete?",
    cancel: true,
    persistent: true,
  }).onOk(() => {
    tasks.value.splice(index, 1);
    $q.notify({
      message: "Task Delete",
      color: "blue",
    });
  });
}
function addTask() {
  tasks.value.push({
    title: newTask.value,
    done: false,
  });
  newTask.value = "";
}
</script>
