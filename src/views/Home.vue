<template>
  <div>
    <navbar class="mb-5 shadow"></navbar>
    <div class="container-fluid">
      <div class="row">

        <!-- backlog -->
        <div class="col">
          <div class="card shadow">
            <h5 class="card-header text-light text-left" style="background-color: #F7580E">Back-Log</h5>
            <div class="card-body p-4">
              <task :tasks="backlog"/>
            </div>
          </div>
        </div>
        <!-- end backlog -->

        <!-- todo -->
        <div class="col">
          <div class="card shadow">
            <h5 class="card-header text-light text-left" style="background-color: #F39215">To-do</h5>
            <div class="card-body p-4">
              <task :tasks="todo"/>
            </div>
          </div>
        </div>
        <!-- end todo -->

        <!-- doing -->
        <div class="col">
          <div class="card shadow">
            <h5 class="card-header text-light text-left" style="background-color: #23BDF3">Doing</h5>
            <div class="card-body p-4">
              <task :tasks="doing"/>
            </div>
          </div>
        </div>
        <!-- end doing -->

        <!-- done -->
        <div class="col">
          <div class="card shadow">
            <h5 class="card-header text-light text-left" style="background-color: #09AD27">Done</h5>
            <div class="card-body p-4">
              <task :tasks="done"/>
            </div>
          </div>
        </div>
        <!-- end done -->

      </div>
    </div>
  </div>
</template>

<script>
import task from "@/components/task.vue";
import navbar from "@/components/navbar.vue";
import db from "@/api/firebase.js";

export default {
  name: "home",
  components: {
    navbar,
    task
  },
  data() {
    return {
      backlog: [],
      todo: [],
      doing: [],
      done: []
    };
  },
  created() {
    this.findAll()
  },
  methods: {
    findAll() {
      db.collection("todos")
      .onSnapshot((doc) => {
          this.backlog = [];
          this.todo = [];
          this.doing = [];
          this.done = [];
          doc.forEach(task => {
            this[`${task.data().status}`].push({
              ...task.data(),
              id: task.id
            });
          });
        },
        (err) => {
          console.log(err);
        }
      );
    }
  },
};
</script>
