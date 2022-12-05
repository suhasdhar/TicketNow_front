<template>
<div >

<div v-if="!deleted">
<div v-if=" saved && !editing "> <h1>SAVED</h1> </div>
<div v-else-if="!editing"><table align="center" >
<tr v-for="(item,index) in this.ticket" :key="index"> <td> {{index}}</td> <td id="elaborate">{{item}}</td></tr>
</table>

<!--  will add comments section here -->
<div>
  <CommentsChat :ticketId="this.ticket.ticketId " :ticket="this.ticket" :id="id"/>
</div>

</div>

<div id="editing" v-else-if="editing">
<input id="left" type="text"  value="Title" disabled>  <input id="right" type="Textbox"  placeholder="title" v-model="this.temp.title" name="" ><br>
<input type="text" id="left"  value="Description" disabled > <input type="Textarea"  placeholder="description"  v-model="this.temp.description" name="" id="description"><br  v-if="this.isadmin===true">

<input id="left" v-if="this.isadmin===true" type="text"  value="Status" disabled >
<select v-if="this.isadmin===true" id="right" name="status" v-model="this.temp.status" >
    <option value="New">New</option>
    <option value="Work in Progress">Work in Progress</option>
    <option value="Completed">Completed</option>
</select> <br v-if="this.isadmin===true">


<input id="left" v-if="this.isadmin===true" type="text"  value="Category" disabled >
<select v-if="this.isadmin===true" id="right" name="category" v-model="this.temp.category"  @change="getServers()">
    <option value="Hardware">Hardware</option>
    <option value="Software">Software</option>
    <option value="Operations">Operations</option>
    <option value="Human Resources">Human Resources</option>


</select> <br v-if="this.isadmin===true">
<input id="left" v-if="this.isadmin===true" type="text" value="Priority" disabled >

<select v-if="this.isadmin===true" id="right" name="priority" v-model="this.temp.priority">
    <option value="P1">P1</option>
    <option value="P2">P2</option>
    <option value="P3">P3</option>
    <option value="P4">P4</option>
  </select> <br>
  <input id="left" v-if="this.isadmin===true" type="text" value="Assigned To" disabled >

<select  v-if="this.isadmin===true" id="right" name="Assignd" v-model="this.temp.idof_assignedTo">
    <option v-for="item in this.listOfWorkers" :value="item" :key="item">{{item}}</option>
  </select> <br>


  <span id='edit' @click="save()">Save</span>

</div>
<span id='delete' @click="del()">Delete</span> <br>
<span id='edit' @click="editOn()">Edit</span>
</div>

<div v-else-if="deleted"> <h1>Ticket Deleted </h1></div>

    </div>
</template>
<script>
import CommentsChat from './CommentsChat.vue';
export default {
  components: {
    CommentsChat
  },
  data () {

    return {
      deleted: false,
      editing: false,
      listOfWorkers: [],
      saved: false,
      temp: {},
      uid: ''

    };

  },

  methods: {
    async del () {

      await fetch(
        'http://localhost:8080/ticket-now/delete-a-ticket',
        {body: JSON.stringify({
        }),
        headers: {'Accept': 'application/json, text/plain',
          'Content-Type': 'application/json;charset=UTF-8',
          'ticketId': this.ticket.ticketId},

        method: 'Delete',
        mode: 'cors'}
      ).then(() => {

        // eslint-disable-next-line
console.log(this.ticket.ticketId)
      });
      this.deleted = true;

    },
    editOn () {

      this.editing = true;
      this.deleted = false;
      this.temp = JSON.parse(JSON.stringify(this.ticket));
      // eslint-disable-next-line
console.log(this.ticket)
    },
    async getServers () {

      this.listOfWorkers = [];
      await fetch(
        'http://localhost:8080/categories/find-user-by-category',
        {headers: {'Accept': 'application/json, text/plain',
          'Content-Type': 'application/json;charset=UTF-8',
          'category': this.temp.category},

        method: 'GET',
        mode: 'cors'}
      ).then((res) => res.json()).
        then((data) => {

          // eslint-disable-next-line guard-for-in
          for (const obj in data) {

            // eslint-disable-next-line
            this.listOfWorkers.push(data[obj].username)

          }


        });

    },
    async save () {

      await fetch(
        'http://localhost:8080/ticket-now/edit-a-ticket',
        {body: JSON.stringify(this.temp),
          headers: {'Accept': 'application/json, text/plain',
            'Content-Type': 'application/json;charset=UTF-8'},

          method: 'PATCH',
          mode: 'cors'}
      );
      this.editing = false;
      this.saved = true;
      // eslint-disable-next-line
      console.log(this.editing+" "+this.saved)

    }
  },

  mounted () {

    this.uid = this.id;

  },
  name: 'LoadTicket',


  props: {
    id: String,
    isadmin: Boolean,
    ticket: Object,
  },
};

</script>

<style>
td{
  border: 1px solid black;
  width: 400px;
  height:10px;
  max-width: 600px;
 overflow: hidden;
 text-overflow: ellipsis;
 padding: 10px;
}
#edit:hover{
  cursor: pointer;
  background-color: rgb(180, 213, 243)
}
#delete:hover{
  cursor: pointer;
  background-color: rgb(239, 16, 16)
}
#description{
  width: 270px;
  max-width: 270px;
 overflow: hidden;
 text-overflow: ellipsis;
 float: right;
}

#description:focus{

  width: 400px;
  overflow-wrap: normal;
  margin-right: 1px;
}
#editing{
  margin: auto;
  width: 40%;
}
#left{
  float: left;
}
#right{
  float: right;
  width:270px;
  max-width: 270px;

}
th{
  border: 1px solid black;
  width: 400px;
  height:10px;
  max-width: 600px;
 overflow: hidden;
 text-overflow: ellipsis;
 padding: 10px;
}
</style>
