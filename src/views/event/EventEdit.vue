<template>
  <div v-if="event">
      <h1>{{ event.title }}</h1>
      <p>Edit the event</p>
  </div>
</template>

<script>
export default {
  props: ['event']
}
</script>
