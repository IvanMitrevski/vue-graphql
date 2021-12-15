<template>
  <ul>
    <li v-for="rocket in rockets" :key="rocket.id">
      <router-link :to="{name: 'Rocket', params: {id: rocket.id}}">
      <h2>{{rocket.name}}</h2>
      </router-link>

    </li>
  </ul>
</template>
<script>
import gql from "graphql-tag"
const query = gql`
query getRockets {
  rockets {
    id
    name
  }
}`

export default {
  apollo: {
    rockets: {
      query
    }
  }
}
</script>

<style scoped>

</style>