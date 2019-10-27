<template>
  <div class="row">
    <div v-if="task" class="col s9 offset-s1">
      <h1>
        {{task.title}}
      </h1>
      <form @submit.prevent="formSubmitHandler">
        <div class="chips" ref="chips"></div>
        <div class="input-field">
          <textarea v-model="description" id="desc" class="materialize-textarea" maxlength="2048"></textarea>
          <label for="desc">
            Description
          </label>
          <span class="character-counter">
              {{description.length}}/2048
            </span>
        </div>
        <label>
          <input type="text" ref="datepicker">
        </label>
        <div v-if="task.status !== 'completed'">
          <button class="btn" type="submit">
            Update
          </button>
          <button class="btn blue" type="button" @click="taskCompleteHandler">
            Complete
          </button>
        </div>
      </form>
    </div>
    <p v-else>
      Task not found
    </p>
  </div>
</template>

<script>
    export default {
        computed: {
            task() {
                return this.$store.getters.taskById(+this.$route.params.id)
            }
        },
        data: () => ({
            description: '',
            chips: null,
            date: null
        }),
        mounted() {
            this.description = this.task.description;
            this.chips = M.Chips.init(this.$refs.chips, {
                placeholder: 'Task tags',
                data: this.task.tags
            });
            this.date = M.Datepicker.init(this.$refs.datepicker, {
                format: 'dd.mm.yyyy',
                defaultDate: new Date(this.task.date),
                setDefaultDate: true
            });
            setTimeout(() => {
                M.updateTextFields()
            }, 0);
        },
        methods: {
            formSubmitHandler() {
                this.$store.dispatch('updateTask', {
                    id: this.task.id,
                    description: this.description,
                    date: this.date.date,
                });
                this.$router.push('/list');
              },
            taskCompleteHandler() {
                this.$store.dispatch('completeTask', this.task.id)
                this.$router.push('/list');
            }
        },
        destroyed() {
            if (this.date && this.date.destroy) {
                this.date.destroy();
            }
            if (this.chips && this.chips.destroy) {
                this.chips.destroy();
            }
        }
    }
</script>

<style scoped>
  .btn.blue {
    margin-left: 15px;
  }
</style>
