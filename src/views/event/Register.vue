<template>
  <h1>{{ event.title }}</h1>
  <p>Register for the event here </p>
</template>

<script>
import EventService from '@/services/EventService.js'
export default {
  props: ['event']
}
</script>
