<!-- eslint-disable max-lines-per-function -->
<template>
<div>
  <div v-if="this.login" > <NavBar v-on:create-new-ticket="disappearTickets($event)" msg="hey"/></div><br>
  <div v-if="this.login === false">
    <img  alt="Ticket Now" src="./assets/ticket.png" />
    <HelloWorld msg="Ticket Now!" />
    <input type="text" placeholder="username" v-model="this.userID">  <input type="password"  placeholder="password"  v-model="this.pass" name="" id=""><br>
    <button :onclick="fun" text="login">login</button>
    <h1 v-if="login" > logged in </h1>
    <h3 v-else-if="failed"> Bad Credentials</h3>
  </div>
  <div v-else>
   <div>
    </div>
    <div  v-if="displayTickets"><CreateTicket msg='view' :role='this.userCode'  :id='this.userCode'/></div>
     <div v-else><CreateTicket msg='create' role='user'  :id="this.userCode" /></div>
     </div>
</div>

</template>
<script>
import CreateTicket from './components/CreateTicket.vue';
import HelloWorld from './components/HelloWorld.vue';
import NavBar from './components/NavBar.vue';
export default {
  components: {
    CreateTicket,
    HelloWorld,
    NavBar,
  },
  computed: {
  },
  data () {

    return {
      data: {
        'suhas': 'Rajul123.',
        // eslint-disable-next-line sort-keys
        'userCode': '',
        // eslint-disable-next-line sort-keys


        // eslint-disable-next-line sort-keys
      },
      displayTickets: true,
      failed: false,
      login: false,
      pass: '',
      userID: '',
      username: '',


    };

  },
  methods: {
    disappearTickets (status) {

      if (status === null) {

        this.login = false;

      }
      this.displayTickets = status;

    },
    async fun () {

      let user = {};
      await fetch(
        'http://localhost:8080/ticket-now/auth/signin',
        {
          body: JSON.stringify({
            'password': this.pass,
            'userId': this.userID,
          }),
          headers: {'Accept': 'application/json, text/plain',
            'Content-Type': 'application/json;charset=UTF-8'},
          method: 'POST',
          mode: 'cors'
        }
      ).then((res) => res.json()).
        then((data) => {

          // eslint-disable-next-line
console.log(this.data);
          user = data;

        }).
        then(() => {


          if (user.usernameAndPasswordMatches) {

            this.login = true;
            this.userCode = this.userID;
            this.displayTickets = true;

          }

        }).
        catch(() => {

          this.failed = true;
          this.login = false;

        });

    }

  },
  name: 'App',
};

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
img {
  border: 1px solid #ddd;
  border-radius: 4px;
  padding: 5px;
  width: 150px;
}
#newticket {

  float: right;
  margin-left: auto;
margin-right: 0;
}
</style>
