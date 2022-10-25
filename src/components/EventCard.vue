<template>
  <router-link class="event-link" :to="{ name: 'EventDetails', params: { id: event.id } }">
    <div class="event-card">
      <span>@{{ event.time }} on {{ event.date }}</span>
      <h4>{{ event.title }}</h4>
    </div>
  </router-link>
</template>

<script>
export default {
  props: {
    event: {
      type: Object,
      required: true
    }
  }
}
</script>

<style scoped>
.event-card {
  padding: 20px;
  width: 250px;
  cursor: pointer;
  margin-bottom: 18px;
  box-shadow: 0 3px 6px 0 rgba(0, 0, 0, 0.2);
  transition-duration: 0.3s;
}

.event-card:hover {
  transform: scale(1.01);
}

.event-link {
  color: #2c3e50;
  text-decoration: none;
}
</style>
