<template>
  <div
    class="task"
    v-for="(task, $taskIndex) of column.tasks"
    :key="$taskIndex"
    draggable
    @dragstart="pickupTask($event, $taskIndex, columnIndex)"
    @click="goToTask(task)"
    @dragover.prevent
    @dragenter.prevent
    @drop.stop="moveTaskOrColumn($event, column.tasks, columnIndex, $taskIndex)"
  >
    <span class="w-full flex-no-shrink font-bold">
      {{ task.name }}
    </span>
    <p v-if="task.description" class="w-full flex-no-shrink mt-1 text-sm">
      {{ task.description }}
    </p>
  </div>
</template>

<script>
export default {
  props: {
    moveTaskOrColumn(e, toTasks, toColumnIndex, toTaskIndex) {
      const type = e.dataTransfer.getData('type');
      if (type === 'task') {
        this.moveTask(
          e,
          toTasks,
          toTaskIndex !== undefined ? toTaskIndex : toTasks.length
        );
      } else {
        this.moveColumn(e, toColumnIndex);
      }
    },
    goToTask(task) {
      this.$router.push({ name: 'task', params: { id: task.id } });
    },
    pickupTask(e, taskIndex, fromColumnIndex) {
      e.dataTransfer.effectAllowed = 'move';
      e.dataTransfer.dropEffect = 'move';
      e.dataTransfer.setData('task-index', taskIndex);
      e.dataTransfer.setData('from-column-index', fromColumnIndex);
      e.dataTransfer.setData('type', 'task');
    }
  },
  methods: {}
};
</script>
<style lang="css" scoped></style>
