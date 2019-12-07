<template>
  <b-container>
    <b-form >
      <b-form-group
        id="input-group-1"
        label="First Name"
        label-for="input-1"
      >
        <b-form-input
          id="input-1"
          v-model="firstName"
          required
          placeholder="Enter First Name"
        ></b-form-input>
      </b-form-group>

      <b-form-group 
        id="input-group-2" 
        label="Last Name:" 
        label-for="input-2"
      >
        <b-form-input
          id="input-2"
          v-model="lastName"
          required
          placeholder="Enter Last Name"
        ></b-form-input>
      </b-form-group>

      <b-form-group 
        id="input-group-3" 
        label="Username:" 
        label-for="input-3"
      >
        <b-form-input
          id="input-3"
          v-model="username"
          required
          placeholder="Enter Username"
        ></b-form-input>
      </b-form-group>      

      <b-form-group 
        id="input-group-4" 
        label="Password:" 
        label-for="input-4"
      >
        <b-form-input
          id="input-4"
          type="password"
          v-model="password"
          required
          placeholder="Enter Password"
        ></b-form-input>
      </b-form-group>
      
      <b-button class="mx-1" v-on:click="restSubmit" variant="primary">Crear con REST </b-button>
      <b-button class="mx-1" v-on:click="graphQLSubmit" variant="secondary">Crear con GraphQL</b-button>
      <b-button class="mx-1" v-on:click="authenticate" variant="danger">Authenticate</b-button>
    </b-form>
    <b-card class="mt-3" header="Output">
      <pre class="m-0">
        {{message}}
      </pre>
      <pre class="m-0">
        {{output}}
      </pre>
    </b-card>
  </b-container>
</template>

<script>

export default {
  name: 'app',
  data() {
    return {
      firstName: '',
      lastName: '',
      username: '',
      password: '',
      message: '',
      output: ''
    };
  },
  methods: {
    restSubmit(e) {
      e.preventDefault();
      let currentObj = this;
      this.axios.post('http://localhost/sa-auth-ms/resources/users', {
        headers: {
          'Access-Control-Allow-Origin': '*'
        },
        firstName: this.firstName,
        lastName: this.lastName,
        username: this.username,
        password: this.password
      })
      .then(function (response) {
        currentObj.message = 'Usuario creado satisfactoriamente desde Microservicio';
        currentObj.output = response.data;
      })
      .catch(function (error) {
        currentObj.message = 'Can\'t access microservice directly'
        currentObj.output = error;
      });
    },

    graphQLSubmit(e){
      e.preventDefault();
      let currentObj = this;
      this.axios.post('http://ec2-18-209-231-31.compute-1.amazonaws.com/graphql',{
        query: `mutation{
          createUser(
            user:{firstName:"${this.firstName}"
             lastName:"${this.lastName}"
              username:"${this.username}"
              password:"${this.password}"}
            ){id firstName lastName username password}}`,
        } 
      ).then(function (response) {
        currentObj.message = 'Usuario creado satisfactoriamente desde API Gateway';
        currentObj.output = response.data;
      })
      .catch(function (error) {
        currentObj.output = error;
      });
    },
    authenticate(e){
      e.preventDefault();
      let currentObj = this;
      this.axios.post('http://ec2-18-209-231-31.compute-1.amazonaws.com/graphql',{
        query: `mutation{
          authenticate(
            user:{
              username:"${this.username}"
              password:"${this.password}"}
            )}`,
        } 
      ).then(function (response) {
        currentObj.message = 'Query Exitosa'
        currentObj.output = response.data;
      })
      .catch(function (error) {
        currentObj.message = 'Error al contactar API'
        currentObj.output = error;
      });
    }
  }
}
</script>
