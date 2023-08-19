<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        v-model="newTask"
        @keyup.enter="addTask"
        class="col"
        square
        filled
        bg-color="white"
        placeholder="Add task"
        danse>

        <template v-slot:append>
          <q-btn
            @click="addTask"
            round
            dense
            flat
            icon="add"/>
        </template>
      </q-input>
    </div>
    <q-list
      class="bg-white"
      separator
      bordered
      >
      <q-item
        v-for="(task, index) in tasks"
        :key="task.title"
        clickable
        @click="task.done = !task.done"
        :class="{'done': task.done}"
        v-ripple>
        <q-item-section avatar>
          <q-checkbox
            v-model="task.done"
            class="no-pointer-events"
            color="primary" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section
          v-if="task.done"
          side
        >
          <q-btn
            @click.stop="deleteTask(index)"
            flat
            round
            color="primary"
            icon="delete"/>
        </q-item-section>
      </q-item>
    </q-list>
    <div
      v-if="!tasks.length"
      class="no-tasks absolute-center">
      <q-icon
        name="check"
        size="100px"
        color="primary"/>
      <div class="text-h5 text-primary text-center">
        No Tasks
      </div>
    </div>
  </q-page>
</template>

<script lang="ts">
import { Dialog, Notify } from 'quasar';
import { defineComponent } from 'vue';
import { Task } from '../interface';

export default defineComponent({
  name: 'TodoPage',
  data() {
    return {
      newTask: '',
      tasks: [

      ] as Task[],
    };
  },
  methods: {
    deleteTask(index: number) {
      Dialog.create({
        title: 'Confirm',
        message: 'Are you sure ?',
        cancel: true,
        persistent: true,
      }).onOk(() => {
        this.tasks.splice(index, 1);
        Notify.create({ message: 'Task deleted successfully!' });
      });
    },
    addTask() {
      this.tasks.push({
        title: this.newTask,
        done: false,
      });
      this.newTask = '';
    },
  },
});
</script>

<style lang="scss">
  .done {
    .q-item__label {
      text-decoration: line-through;
      color: #bbb;
    }
  }
</style>
