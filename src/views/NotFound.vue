<template>
<div>
    <h1>Oops!</h1>
    <h2>The {{resource}} you're looking for is not here.</h2>
    <router-link :to="{ name: 'EventList' }">Go back home</router-link>
</div>
</template>

<script>
export default {
  props: {
    resource: {
      type: String,
      required: true,
      default: 'page'
    }
  }
}
</script>
