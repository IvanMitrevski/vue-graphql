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
import {GET_USERS} from "@/queries";

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
        },
        update: (cache, {data: {insert_users}}) => {
          const [newUser] = insert_users.returning;
          const data = cache.readQuery({query: GET_USERS})
          data.users.unshift(newUser)
          data.users.pop()
          cache.writeQuery({query: GET_USERS, data})
        },
        optimisticResponse: {
          __typename: "Mutation",
          insert_users: {
            __typename: "users_mutation_response",
            returning: [
              {
                __typename: "users",
                id: -1,
                name,
                twitter,
                rocket
              }
            ]
          }
        }
      })
          .then(data => {
            console.log("User data: ", data)
          })
          .catch(error => {
            console.log(error)
          })
          .then(() => {
            Object.assign(this.$data, {
              name: '',
              twitter: '',
              rocket: ''
            })
          })
    }
  }
}
</script>

<style scoped>

</style>