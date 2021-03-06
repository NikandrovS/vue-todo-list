<template>
  <div class="row">
    <div v-if="task" class="col s6 offset-s3">
      <h1>{{ task.title }}</h1>

      <form v-on:submit.prevent="submitHandler">
        <div class="input-field">
          <textarea v-bind:="{disabled: task.status === 'completed'}" style="min-height: 100px" v-model="description" id="description" class="materialize-textarea" maxlength="400"></textarea>
          <label for="description">Description</label>
          <span class="character-counter" style="float: right; font-size: 12px;">{{ description.length }}/400</span>
        </div>

        <div class="chips" ref="chips" ></div>

        <input type="text" ref="datepicker">
        <div v-if="task.status !== 'completed'">
          <button class="btn" type="submit">Update task</button>
          <button class="btn blue" type="button" v-on:click="completeTask">Complete task</button>
        </div>
      </form>
    </div>
    <p v-else>Task not found</p>
  </div>
</template>

<script>
export default {
  name: "Task",
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
    this.description = this.task.description
    this.chips = M.Chips.init(this.$refs.chips, {
      placeholder: 'Task tags',
      data: this.task.tags,
      limit: this.task.status === 'completed' ? 0 : Infinity
    })
    this.date = M.Datepicker.init(this.$refs.datepicker, {
      format: 'dd.mm.yyyy',
      defaultDate: new Date(this.task.date),
      setDefaultDate: true
    })
    setTimeout(() => {
      M.updateTextFields()
    }, 0)
  },
  methods: {
    submitHandler() {
      this.$store.dispatch('updateTask', {
        id: this.task.id,
        description: this.description,
        date: this.date.date
      })
      this.$router.push('/list')
    },
    completeTask() {
      this.$store.dispatch('completeTask', this.task.id)
      this.$router.push('/list')
    }
  },
  unmounted() {
    if (this.date && this.date.destroy) {
      this.date.destroy
    }
    if (this.chips && this.chips.destroy) {
      this.chips.destroy
    }
  }
}
</script>

<style scoped>
  textarea {
    overflow-y: auto;
  }
  button[type='submit'] {
    margin-right: 1rem;
  }
</style>