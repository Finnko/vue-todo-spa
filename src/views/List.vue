<template>
  <div>
    <h1>
      List
    </h1>
    <div class="row">
      <div class="input-field col s5">
        <select ref="select" id="task-select" v-model="filter">
          <option value="" disabled selected>Choose status</option>
          <option value="active">Active</option>
          <option value="outdated">Outdated</option>
          <option value="completed">Completed</option>
        </select>
        <label for="task-select">Filters</label>
      </div>
    </div>
    <table v-if="tasks.length">
      <thead>
      <tr>
        <th>#</th>
        <th>Title</th>
        <th>Date</th>
        <th>Description</th>
        <th>Status</th>
        <th>View Task</th>
      </tr>
      </thead>
      <tbody>
        <tr
            v-for="(task, idx) of filteredTasks"
            :key="task.id"
        >
          <td>{{idx + 1}}</td>
          <td>{{task.title}}</td>
          <td>{{new Date(task.date).toLocaleDateString()}}</td>
          <td class="task-desc">
            <p class="task-desc__text">
              {{task.description}}
            </p>
          </td>
          <td>{{task.status}}</td>
          <td>
            <router-link tag="button" class="btn btn-small" :to="'/task/' + task.id">
              View
            </router-link>
          </td>
        </tr>
      </tbody>
    </table>
    <p v-else>
      No avaliable tasks
    </p>
  </div>

</template>

<script>
    export default {
      data: () => ({
          filter: null,
      }),
      computed: {
          tasks() {
              return this.$store.getters.tasks
          },
          filteredTasks() {
              return this.tasks.filter(task => {
                  if (!this.filter) {
                      return true;
                  }
                  return task.status === this.filter;
              })
          }

      },
      mounted() {
          M.FormSelect.init(this.$refs.select);
      }
    }
</script>

<style scoped>
  .task-desc {
    max-width: 375px;
  }

  .task-desc__text {
    margin: 0;
    white-space: nowrap;
    text-overflow: ellipsis;
    overflow-x: hidden;
  }
</style>
