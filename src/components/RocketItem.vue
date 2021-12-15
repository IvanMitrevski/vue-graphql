<template>
  <div class="rocket-item">
    <div v-if="rocket">
      <h2>🚀 {{rocket.name}}</h2>
      <h2>{{rocket.description}}</h2>
      <h2>$ {{rocket.costPerLaunch}}</h2>
    </div>
  </div>
</template>
<script>
import gql from "graphql-tag"
const query = gql`
query getRocket($id: ID!) {
  rocket(id: $id) {
    id
    name
    description
    costPerLaunch: cost_per_launch
  }
}`

export default {
  apollo: {
    rocket: {
      query,
      variables() {
        return {
          id: this.$route.params.id
        }
      }
    }
  }
}
</script>

<style scoped>

</style>