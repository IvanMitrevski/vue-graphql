<template>
  <div class="user-form">
    <form @submit.prevent="submit">
      <fieldset>
        <input type="text" placeholder="Name" v-model="name" required/>
        <input type="text" placeholder="Twitter" v-model="twitter" required/>
        <select v-model="rocket" required>
          <option :selected="true" :value="null">Choose Rocket</option>
          <option v-for="name in ['Falcon 1', 'Falcon 9', 'Falcon Heavy']" :key="name" :value="name">
            {{ name }}
          </option>
        </select>
        <input type="submit" value="Send"/>
      </fieldset>
    </form>

  </div>
</template>

<script>
import {v4 as uuidv4} from 'uuid';
import {INSERT_USER} from "../mutations";

export default {
  name: "UserForm",
  data() {
    return {
      name: '',
      twitter: '',
      rocket: ''
    }
  },
  methods: {
    submit() {
      const {name, twitter, rocket} = this.$data
      const id = uuidv4()
      this.$apollo.mutate({
        mutation: INSERT_USER,
        variables: {
          id,
          name,
          twitter,
          rocket
        }
      })
    }
  }
}
</script>

<style scoped>

</style>