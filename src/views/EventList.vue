<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <div class="events">
    <h1>Event for good</h1>

    <EventCard v-for="event in events" :key="event.id" :event="event" />

    <div class="pagination">
      <div class="btn" v-if="page != 1">
        <router-link id="page-prev" :to="{ name: 'EventList', query: { page: page - 1 } }" rel="prev">&#60;</router-link>
      </div>
      <div class="btn" v-for="pageNum in totalPages" :key="pageNum">
        <router-link :class="page === pageNum ? 'activePage' : ''" :to="{ name: 'EventList', query: { page: pageNum } }" :rel="pageNum">{{pageNum}}</router-link>
      </div>
      <div class="btn" v-if="hasNextPage">
        <router-link id="page-nxt" :to="{ name: 'EventList', query: { page: page + 1 } }" rel="prev">&#62;</router-link>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import EventCard from '@/components/EventCard.vue'
import EventService from '@/services/EventService'

export default {
  name: 'EventList',
  props: ['page'],
  components: {
    EventCard
  },
  data () {
    return {
      events: null,
      totalEvents: 0
    }
  },
  computed: {
    totalPages () {
      return Math.ceil(this.totalEvents / 2)
    },
    hasNextPage () {
      return this.page < this.totalPages
    }
  },
  beforeRouteEnter (to, from, next) {
    EventService.getEvents(2, parseInt(to.query.page) || 1)
      .then(response => {
        next(comp => {
          comp.totalEvents = response.headers['x-total-count']
          comp.events = response.data
        })
      })
      .catch(error => {
        if (error.response && error.response.status === 404) {
          next({
            name: '404Resource',
            params: { resource: 'event' }
          })
        } else {
          next({
            name: 'NetworkError'
          })
        }
      })
  },
  beforeRouteUpdate (to) {
    return EventService.getEvents(2, parseInt(to.query.page) || 1)
      .then(response => {
        this.totalEvents = response.headers['x-total-count']
        this.events = response.data
      })
      .catch(error => {
        if (error.response && error.response.status === 404) {
          return {
            name: '404Resource',
            params: { resource: 'event' }
          }
        } else {
          return {
            name: 'NetworkError'
          }
        }
      })
  }
}
</script>

<style scoped>
.events {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.pagination {
  display: flex;
  width: 290px;
  justify-content: space-between;
  margin-top: 20px;
}
.pagination .btn {
  flex: 1;
}
.pagination a {
  background-color: rgb(202, 202, 202);
  border-radius: 50%;
  text-decoration: none;
  color: #424242;
  font-weight: bold;
  padding: 10px 15px;
}
.activePage {
  background-color: rgb(19, 115, 241) !important;
  color: #fff !important;
}
#page-prev {
  text-align: left;
}
#page-next {
  text-align: right;
}
</style>
