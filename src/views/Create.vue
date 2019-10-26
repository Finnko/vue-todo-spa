<template>
  <div>
    <div class="row">
      <div class="col s6 offset-s3">
        <h1>
          Create task
        </h1>
        <form @submit.prevent="formSubmitHandler">
          <div class="input-field">
            <input v-model="title" id="title" type="text" class="validate" required>
            <label for="title">
              Title
            </label>
            <span class="helper-text" data-error="Title is required"></span>
          </div>
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
          <button class="btn" type="submit">
            Create task
          </button>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'create',
  data: () => ({
      description: '',
      title: '',
      chips: null,
      date: null
  }),
  mounted() {
      this.chips = M.Chips.init(this.$refs.chips, {
          placeholder: 'Task tags'
      });
      this.date = M.Datepicker.init(this.$refs.datepicker, {
          format: 'dd.mm.yyyy',
          defaultDate: new Date(),
          setDefaultDate: true
      });
  },
  methods: {
    formSubmitHandler() {
        const task = {
            title: this.title,
            description: this.description,
            id: Date.now(),
            status: 'active',
            tags: this.chips.chipsData,
            date: this.date.date,
        };

        this.$store.dispatch('createTask', task);
        this.$router.push('/list');
    },
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

<style lang="scss">
  .input-field .character-counter {
    text-align: center;
  }

  .character-counter {
    font-size: 12px;
  }
</style>
