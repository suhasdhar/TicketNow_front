<!-- eslint-disable max-lines-per-function -->
<template>
  <div> <img  class="bg" src="./assets/service-tool-icon-on-white-background-vector-22746653.jpg" />
    <div id="app">
  <div v-if="this.login" > <NavBar v-on:create-new-ticket="disappearTickets($event)" msg="hey" :isadmin="this.isAdmin"/> </div><br>
 <h2>Welcome <b>{{this.username}}</b></h2>
  <div v-if="this.login === false">
    <img  alt="Ticket Now" src="./assets/ticket.png" />
    <HelloWorld msg="Ticket Now!" />
    <input type="text" placeholder="username" v-model="this.userID">  <input type="password"  placeholder="password"  v-model="this.pass" name="" id=""><br>
    <button :onclick="fun" text="login">login</button>
    <h1 v-if="login" > logged in </h1>
    <h3 v-else-if="failed" id="alert"> Bad Credentials</h3>
  </div>
  <div v-else-if="(!this.adminportallogin && this.login === true )">
    <div  v-if="displayTickets"><CreateTicket msg='view' :admin='this.isAdmin'  :id='this.userCode'/></div>
     <div v-else><CreateTicket msg='create' :admin='this.isAdmin'  :id="this.userCode" /></div>
     </div>
     <div v-else > <AdminPortal v-on:AdminPortalValue="AdminPortalOn($emit)"/> </div>
</div>

  </div>

</template>
<script>
import AdminPortal from './components/AdminPortal.vue';
import CreateTicket from './components/CreateTicket.vue';
import HelloWorld from './components/HelloWorld.vue';
import NavBar from './components/NavBar.vue';

export default {
  components: {
    AdminPortal,
    CreateTicket,
    HelloWorld,
    NavBar,
  },
  computed: {
  },
  data () {

    return {
      adminportallogin: false,
      data: {
        'suhas': 'Rajul123.',
        // eslint-disable-next-line sort-keys
        'userCode': '',
        // eslint-disable-next-line sort-keys


        // eslint-disable-next-line sort-keys
      },
      displayTickets: true,
      failed: false,
      isAdmin: false,
      login: false,
      pass: '',
      userID: '',
      username: '',


    };

  },
  methods: {
    AdminPortalOn (status) {

      // eslint-disable-next-line
console.log(status)
      this.adminportallogin = false;

    },
    disappearTickets (status) {

      if (status === 'Admin-portal') {

        this.adminportallogin = true;

      }

      if (status === null) {

        this.login = false;
        this.failed = false;
        this.isAdmin = false;
        this.adminportallogin = false;

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

          user = data;

        }).
        then(() => {

          // eslint-disable-next-line no-magic-numbers
          if (user.roles[0] === 'ROLE_ADMIN') {

            this.isAdmin = true;
            // eslint-disable-next-line
              console.log(user.roles[0])
          }
          if (user.usernameAndPasswordMatches) {

            this.login = true;
            this.userCode = this.userID;
            this.displayTickets = true;
            this.username = user.username;

          }

        }).
        catch(() => {

          this.failed = true;
          this.login = false;

        });
      // eslint-disable-next-line
      //  console.log(this.isAdmin)
    }

  },
  name: 'App',
};

</script>

<style : scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
#headerImage{
  position: absolute;
      right: 0px;
      height: 30px;

}
img.bg {
      position: absolute;
      left: 0px;
      top: 0px;
      z-index: -1;
      width: 100%;
      height: 100%;
      -webkit-filter: blur(30px); /* Safari 6.0 - 9.0 */
      filter: blur(100px);
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
#alert{color: red;}

</style>
