<template>
  <div @dblclick="$emit('toggle-reminder', task.id)" :class="[task.reminder && 'reminder', 'task']">
    <h3>
      {{ task.text }}
      <i @click="handleDelete(task.id)" class="far fa-trash-alt" />
    </h3>
    <p>{{ task.day }}</p>
  </div>
</template>

<script>
export default {
  name: "Task",
  props: {
    task: Object,
  },
  methods:{
    handleDelete(id){ this.$emit('delete-task', id)}
  },
  emits: ['delete-task', 'toggle-reminder']
};
</script>
<style scoped>
.far.fa-trash-alt {
  color: red;
}
.task {
  background: #f4f4f4;
  margin: 5px;
  padding: 10px 20px;
  cursor: pointer;
}
.task.reminder {
  border-left: 5px solid green;
}
.task h3 {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
</style>