<template>
  <div class="container">
    <div class="row justify-content-center">
            <div class="col-md-8">
                <div class="card">
                    <div class="card-header">Conexión API-Rest </div>
                    <div class="card-body">
                        <form @submit="restSubmit">
                        <strong>firstName:</strong>
                        <input type="text" class="form-control" v-model="firstName">
                        <strong>lastName:</strong>
                        <input type="text" class="form-control" v-model="lastName">
                        <strong>username:</strong>
                        <input type="text" class="form-control" v-model="username">
                        <strong>password:</strong>
                        <input type="text" class="form-control" v-model="password">
                        <button class="btn btn-success">Submit</button>
                        </form>
                        <strong>Output:</strong>
                        <pre>
                        {{output}}
                        </pre>
                    </div>
                </div>
            </div>
        </div>

        <div class="row justify-content-center">
            <div class="col-md-8">
                <div class="card">
                    <div class="card-header">Conexión API-GraphQL </div>
                    <div class="card-body">
                        <form @submit="graphQLSubmit">
                        <strong>firstName:</strong>
                        <input type="text" class="form-control" v-model="firstName">
                        <strong>lastName:</strong>
                        <input type="text" class="form-control" v-model="lastName">
                        <strong>username:</strong>
                        <input type="text" class="form-control" v-model="username">
                        <strong>password:</strong>
                        <input type="text" class="form-control" v-model="password">
                        <button class="btn btn-success">Submit</button>
                        </form>
                        <strong>Output:</strong>
                        <pre>
                        {{output}}
                        </pre>
                    </div>
                </div>
            </div>
        </div>

    </div>
</template>

<script>
import gql from 'graphql-tag'

export default {
  name: 'app',
  data() {
    return {
      firstName: '',
      lastName: '',
      username: '',
      password: '',
      output: ''
    };
  },
  methods: {
    restSubmit(e) {
      e.preventDefault();
      let currentObj = this;
      this.axios.post('http://localhost:4000/sa-auth-ms/resources/users', {
        headers: {
          'Access-Control-Allow-Origin': '*'
        },
        firstName: this.firstName,
        lastName: this.lastName,
        username: this.username,
        password: this.password
      })
      .then(function (response) {
        currentObj.output = response.data;
      })
      .catch(function (error) {
        currentObj.output = error;
      });
    },

    graphQLSubmit(){
          this.$apollo.mutate({
              mutation: gql`mutation createUser($firstName: String!, $lastName: String!, $username: String!, $password: String!){
                createUser(firstName: $firstName, lastName: $lastName, username: $username, password: $password) {
                  firstName,
                  lastName,
                  username,
                  password}
              }`,
              variables:{
                firstName: this.firstName,
                lastName: this.lastName,
                username: this.username,
                password: this.password
              }
            }
          )
          location.reload();
        },
  }
}
</script>
