<template>
  <div>
    <div class="card" v-for="task in tasks" :key="task.id">
      <div class="card-header" style="background-color: white"><strong>{{ task.title }}</strong></div>
      <div class="card-body" style="color: #C2C5C3">
        <p class="card-text">Task Description: {{ task.description }}</p>
        <p class="card-text">Point: {{ task.point }}</p>
        <p class="card-text">Assign To: {{ task.assignTo }}</p>
        <div class="d-flex justify-content-around">
          <button class="btn btn-danger btn-sm text-light" @click.prevent="moveTask(task.id, task.status, -1)" v-if="task.status == 'todo'">Back log</button>
          <button class="btn btn-warning btn-sm text-light" @click.prevent="moveTask(task.id, task.status, -1)" v-if="task.status == 'doing'">To Do</button>
          <button class="btn btn-info btn-sm text-light" @click.prevent="moveTask(task.id, task.status, -1)" v-if="task.status == 'done'">Doing</button>
          <button class="btn btn-outline-danger btn-sm" style="background-color: #00ffcc; color: white; border: 1px solid #00ffcc" @click.prevent="deleteTask(task.id)">Delete</button>
          <button class="btn btn-warning btn-sm text-light" @click.prevent="moveTask(task.id, task.status, 1)" v-if="task.status == 'backlog'">To Do</button>
          <button class="btn btn-info btn-sm text-light" @click.prevent="moveTask(task.id, task.status, 1)" v-if="task.status == 'todo'">Doing</button>
          <button class="btn btn-success btn-sm text-light" @click.prevent="moveTask(task.id, task.status, 1)" v-if="task.status == 'doing'">Done</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import db from '@/api/firebase.js';

export default {
  name: 'task',
  props: ['tasks'],
  methods: {
    moveTask(id, status, direction) {
      const statusList = ['backlog', 'todo', 'doing', 'done'];
      db.collection('todos')
        .doc(id)
        .update({
          status: statusList[statusList.indexOf(status) + direction],
        })
        .then(() => {
          console.log('success move');
        })
        .catch((err) => {
          console.log(err);
        });
    },
    deleteTask(id) {
      db.collection('todos')
        .doc(id)
        .delete()
        .then(() => {
          console.log('success delete');
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
};
</script> 