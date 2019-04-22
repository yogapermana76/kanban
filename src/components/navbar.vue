<template>
  <div>
    <!-- navbar -->
    <nav class="navbar justify-content-between">
      <a class="navbar-brand"><h3>Kanban Board</h3></a>
      <button
        class="btn btn-outline-info my-2 my-sm-0"
        data-toggle="modal"
        data-target="#modalTask"
      >Add Task</button>
    </nav>
    <!-- end navbar -->

    <!-- modal new task -->
    <div class="modal fade" id="modalTask" tabindex="-1" role="dialog" aria-hidden="true">
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title">New Task</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <form @submit.prevent="newTask">
              <div class="form-group">
                <label class="col-form-label">Title</label>
                <input type="text" class="form-control" v-model="title" placeholder="Task Title" required>
              </div>
              <div class="form-group">
                <label class="col-form-label">Description</label>
                <textarea class="form-control" v-model="description" placeholder="Task Sort Description" required></textarea>
              </div>
              <div class="form-group">
                <label class="col-form-label">Point</label>
                <input
                  type="number"
                  min="1"
                  max="100"
                  class="form-control"
                  v-model="point"
                  placeholder="0"
                  required
                >
              </div>
              <div class="form-group">
                <label class="col-form-label">Assigned To</label>
                <input type="text" class="form-control" v-model="assignTo" placeholder="Assign To" required>
              </div>
              <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-dismiss="modal"
                  style="background-color: #FC419C; color: white; border: 1px solid #FC419C"
                >Cancel</button>
                <button type="submit" class="btn btn-primary"
                  style="background-color: #00ffcc; color: white; border: 1px solid #00ffcc"
                >Add</button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
    <!-- end modal new task -->

  </div>
</template>

<script>
import db from "@/api/firebase.js";

export default {
  name: "navbar",
  data() {
    return {
      title: '',
      point: '',
      description: '',
      assignTo: ''
    };
  },
  methods: {
    newTask() {
      db.collection("todos")
        .add({
          title: this.title,
          point: this.point,
          description: this.description,
          assignTo: this.assignTo,
          status: "backlog"
        })
        .then(doc => {
          this.title = '';
          this.point = '';
          this.description = ''
          this.assignTo = '';
          console.log(doc);
        })
        .catch(err => {
          console.log(err);
        });
        $('#modalTask').modal('toggle');
    }
  }
};
</script>