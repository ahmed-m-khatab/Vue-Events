<template>
  <div v-if="event">
      <h1>{{ event.title }}</h1>
      <p>Register the event</p>
      <button @click="register">Register</button>
  </div>
</template>

<script>
export default {
  props: ['event'],
  inject: ['GStore'],
  methods: {
    register () {
      this.GStore.flashMessage = 'You are registered for ' + this.event.title

      setTimeout(() => {
        this.GStore.flashMessage = ' '
      }, 3000)

      this.$router.push({ name: 'EventDetails' })
    }
  }
}
</script>
