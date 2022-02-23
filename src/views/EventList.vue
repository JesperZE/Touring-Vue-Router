<template>
  <h1>Events for Good</h1>
  <div class="events">
    <EventCard v-for="event in events" :key="event.id" :event="event" />
    <router-link
    :to="{ name: 'EventList', query: { page: page - 1 } }"
    rel="prev"
    v-if="page != 1"
    >Previous Page</router-link>

    <router-link
    :to="{ name: 'EventList', query: {page: + 1 } }"
    rel="next"
    v-if="hasNextPage"
    >Next Page
    </router-link>

  </div>

</template>

<script>
import EventCard from '@/components/EventCard.vue'
import EventService from '@/services/EventService.js'
import { watchEffect } from 'vue'

export default {
  name: 'EventList',
  props: ['page'],
  components: {
    EventCard
  },
  data() {
    return {
      events: null,
      totalEvents: 0, // <- Store totalEvents that are fetched from the api

    }
  },
  created() {
      watchEffect(() => {
        this.events = null 
        EventService.getEvents(2, this.page)
          .then(response => {
            this.events = response.data
            this.totalEvents = response.headers['x-total-count']
          })
          .catch(error => {
            console.log(error)
          })
      })
  },
  computed: {
    hasNextPage() {
      // Calculate the total pages
      var totalPages = Math.ceil(this.totalEvents / 2) // 2 events per page
      
      // Return true if the current page is less than the total pages
      return this.page < totalPages
    }
  }
}
</script>

<style scoped>
.events {
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>
