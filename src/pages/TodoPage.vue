<template>
  <q-page class="bg-grey-3 column">
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
  </q-page>
</template>

<script lang="ts">
import { Dialog, Notify } from 'quasar';
import { defineComponent } from 'vue';

interface Task {
  title: string;
  done: boolean;
}

export default defineComponent({
  data() {
    return {
      tasks: [
        {
          title: 'Get bananas',
          done: false,
        },
        {
          title: 'Eat bananas',
          done: false,
        },
        {
          title: 'Poo bananas',
          done: false,
        },
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
