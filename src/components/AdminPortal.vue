<!-- eslint-disable max-statements -->
<template>
<div>
Admin Portal
 <br>

<div > <button @click="this.createUser()" >Create a user</button> <button @click="this.deleteUser()">Delete a user</button> <br><br><br> </div>

 <!-- creating a user -->
<div v-if="this.create">


<input type="text"  value="Username" disabled> <input type="Textbox"  placeholder="Full Name"  v-model="this.username" name="" id=""><br>

<input type="text"  value="Userid" disabled > <input type="Textarea" @change="validate()" placeholder="Alphanumeric without space"  v-model="this.userid" name="" id=""> <br>
<div v-if="this.validuser!=null && !this.validuser" id="alert"> The userid already exists <br></div>
<div v-if="this.validuser!=null && this.validuser" id="green"> Valid Userid <br></div>
<input   type="text"  value="Category" disabled >

<select id="Category" name="category" v-model="this.category">
    <option value="Hardware">Hardware</option>
    <option value="Software">Software</option>
    <option value="Operations">Operations</option>
    <option value="Human Resources">Human Resources</option>
    <option value="Human Resources">User</option>

</select> <br >
<input   type="text"  value="Is Admin" disabled >
<select id="Category" name="category" v-model="this.isadmin">
    <option value="Yes">Yes</option>
    <option value="No">No</option>
</select>
<br>
<input  type="text" value="Password" disabled >

<input type="password" v-model="this.password"> <br>
<br>
<button @click="this.addusertodatabase()"> Submit</button>
<div v-if="this.vusercreated" id="green"> User Created </div>

</div>

<!-- deleting a user -->
<div v-if="(this.create===false)">
   <input type="text" value="userid" disabled>  <input type="text" v-model="this.userid" >

   <br>

   <button  @click="deleteuserfromdatabase()" >delete user</button>
   <div v-if="this.vuserdeleted" id="alert"> User Deleted </div>

</div>
<br>
<button @click="$emit('AdminPortalValue',false)"> Back </button>

    </div>
</template>
<script>
export default {


  data () {

    return {
      category: '',
      create: null,
      isadmin: null,
      name: 'AdminPortal',
      password: '',
      userid: '',
      username: '',
      validuser: null,
      vusercreated: null,
      vuserdeleted: null,

    };

  },
  methods: {
    // eslint-disable-next-line max-statements
    async addusertodatabase () {

      if (this.userid === '') {

        this.validuser = false;
        return;

      }
      this.validate();
      if (this.validuser) {

        const user = {};
        user.category = this.category;
        user.isadmin = this.isadmin;
        user.userid = this.userid;
        user.username = this.username;
        user.password = this.password;
        await fetch(
          'http://localhost:8080/ticket-now/auth/add-user',
          {
            body: JSON.stringify({
              'password': user.password,
              'userId': user.userid,
              'username': user.username,
            }),
            headers: {'Accept': 'application/json, text/plain',
              'Content-Type': 'application/json;charset=UTF-8',
              'category': user.category,
              'isadmin': user.isadmin},
            method: 'POST',
            mode: 'cors'
          }
        ).then((res) => res.json());

        this.vusercreated = true;

      }

    },
    createUser () {

      this.create = true;
      this.userid = null;
      this.validuser = null;
      this.vusercreated = false;

    },
    deleteUser () {

      this.create = false;
      this.userid = null;
      this.vuserdeleted = false;

    },
    async deleteuserfromdatabase () {

      // eslint-disable-next-line
      console.log(this.userid+" **");
      await fetch(
        'http://localhost:8080/ticket-now/auth/delete-user',
        {
          headers: {'Accept': 'application/json, text/plain',
            'Content-Type': 'application/json;charset=UTF-8',
            'userid': this.userid},
          method: 'DELETE',
          mode: 'cors'
        }
      ).then((res) => res.json()).
        catch((error) => {

          // eslint-disable-next-line
      console.log(error)
        });

      this.vuserdeleted = true;

    },
    async validate () {

      if (this.userid === '') {

        this.validuser = false;
        return;

      }
      await fetch(
        'http://localhost:8080/ticket-now/auth/validate-user',
        {
          headers: {'Accept': 'application/json, text/plain',
            'Content-Type': 'application/json;charset=UTF-8',
            'userid': this.userid},
          method: 'POST',
          mode: 'cors'
        }
      ).then((res) => res.json()).
        then((data) => {

          if (data) {

            this.validuser = false;
            // eslint-disable-next-line
             console.log(this.validuser)
          } else {

            this.validuser = true;

          }

        });


    },
  },
  mounted () {

    this.validuser = null;
    this.create = null;

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
#validate{
    width:60px
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
#green{
  color: rgb(2, 113, 2);
}
</style>
