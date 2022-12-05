<template>
<div>
<!--if the msg prop is create the below code renders-->
<div  v-if="this.msg==='create' && !creationDone">
<input type="text"  value="Title" disabled> <input type="Textbox"  placeholder="title"  v-model="title" name="" id=""><br>

<input type="text"  value="Description" disabled > <input type="Textarea"  placeholder="description"  v-model="description" name="" id=""><br  v-if="this.admin===true">

<input  v-if="this.admin===true" type="text"  value="Category" disabled >

<select v-if="this.admin===true" id="Category" name="category" v-model="category">
    <option value="Hardware">Hardware</option>
    <option value="Software">Software</option>
    <option value="Operations">Operations</option>
    <option value="Human Resources">Human Resources</option>

</select> <br v-if="this.admin===true">

<input v-if="this.admin===true" type="text" value="Priority" disabled >

<select  v-if="this.admin===true" id="Category" name="priority" v-model="priority">
    <option value="P1">P1</option>
    <option value="P2">P2</option>
    <option value="P3">P3</option>
    <option value="P4">P4</option>
  </select> <br>
<!-- <input type="text"  value="Creation Date" disabled > <input type="Date"  placeholder="creationDate"  v-model="creationDate" name="" id=""><br>
<input type="text"  value="Creation time" disabled > <input type="Time"  placeholder="creationTime"  v-model="creationTime" name="" id=""><br> -->
<div v-if="alert" id="alert"> Title and Description cant be empty </div>
<button @click="create()">Submit</button>
</div>

<div  v-else-if="this.msg==='create' && creationDone"> Ticket created click on view ticket to see it</div>

<div v-else>
  <table v-if="!this.viewTicket">
  <tr> <th>Ticket ID</th> <th>Title</th><th>Status</th><th>Description</th><th>Category</th></tr>
  <tr id='row' v-for="(item) in fetched" :key="item.userid" @click="view(item)"> <td>{{item.ticketId}} </td><td>{{item.title}} </td><td id="Status">{{item.status}} </td> <td>{{item.description}} </td><td>{{item.category}} </td></tr>
  </table>
  <div v-else>
    <span id='back' @click="back()">Back</span>
    <LoadTicket :ticket="this.selected" :isadmin="admin" :id="this.id" />
  </div>
</div>

</div>
</template>

<script>
import LoadTicket from './LoadTicket.vue';
export default {
  components: {
    LoadTicket
  },
  data () {

    return {
      alert: false,
      category: '',
      creationDone: false,
      description: null,
      fetched: '',
      priority: '',
      rawTicket: {'description': '',
        'priority': '',
        'title': ''},
      selected: '',
      title: null,
      viewTicket: false,

    };

  },
  name: 'CreateTicket',
  props: {
    admin: Boolean,
    id: String,
    msg: String,

  },
  // eslint-disable-next-line sort-keys
  methods: {
    back () {

      this.viewTicket = false;
      this.gettickets();

    },
    async create () {

      this.rawTicket.title = this.title;
      this.rawTicket.description = this.description;
      this.rawTicket.priority = this.priority;
      if (this.title === null || this.description === null) {

        this.alert = true;
        return;

      }
      // eslint-disable-next-line
      console.log(typeof this.priority);
      await fetch(
        'http://localhost:8080/ticket-now/add-new-ticket',
        {body: JSON.stringify({
          'description': this.description,
          'idof_createdBy': this.id,
          'priority': this.priority,
          'title': this.title,
        }),
        headers: {'Accept': 'application/json, text/plain',
          'Content-Type': 'application/json;charset=UTF-8'},
        method: 'POST',
        mode: 'cors'}
      ).then((res) => res.json()).
      // eslint-disable-next-line
        catch((error) => console.log(error));
      this.creationDone = true;

    },
    async gettickets () {

      if (this.admin) {

        await fetch(
          'http://localhost:8080/ticket-now/admin/fetch-all-tickets',
          {
            method: 'GET',
            mode: 'cors'
          }
        ).then((res) => res.json()).
          then((data) => {

            this.fetched = data;
            // eslint-disable-next-line
            console.log(this.admin);

          });

      } else {

        await fetch(
          'http://localhost:8080/ticket-now/fetch-all-tickets',
          {
            headers: {'Accept': 'application/json,text/plain',
              'Content-Type': 'application/json;charset=UTF-8',
              'userid': this.id},
            method: 'POST',
            mode: 'cors'
          }
        ).then((res) => res.json()).
          then((data) => {

            this.fetched = data;

          }).
          catch((error) => {

            // eslint-disable-next-line
          console.log(error);
          });

      }

    },
    view (item) {

      this.selected = item;
      this.viewTicket = true;
      // eslint-disable-next-line
      console.log(this.selected)

    },

  },
  mounted () {

    if (this.msg === 'view') {

      // eslint-disable-next-line
      this.gettickets();
      this.viewTicket = false;

    }
    // eslint-disable-next-line
    console.log(this.id+" ***")

  },
};

</script>

<style scoped>
input{

  width : 200px;
}
select{

width : 200px;
}
button{

width : 200px;
}
table{
  border: solid;
 align-content: center;
}
td {
  border: 1px solid black;
  width: 100px;
  height:10px;
  max-width: 600px;
 overflow: hidden;
 text-overflow: ellipsis;
 white-space: nowrap;
 padding: 10px;
}

#row:hover {
  cursor: pointer;
  background-color: rgb(180, 213, 243)
}
#back:hover {
  cursor: pointer;
  background-color: rgb(180, 213, 243)
}
#alert{
  color: red;
}
</style>
