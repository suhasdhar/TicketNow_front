<template>
<div id="bar">  <button @click="$emit('create-new-ticket',false)" id="Create_Ticket"> Create ticket</button> <button @click="$emit('create-new-ticket',true)" id="Create_Ticket"> View tickets</button>
  <button @click="$emit('create-new-ticket','Admin-portal')" id="Create_Ticket" v-if="isadmin"> Admin Portal</button> <button @click="$emit('create-new-ticket',null)" id="Create_Ticket"> Sign out</button>
<img  alt="Ticket Now" src="../ticket.png" /> <br></div>
</template>
<script>

export default {
  name: 'NavBar',
  props: {
    isadmin: Boolean,
    msg: String,
  },
  // eslint-disable-next-line sort-keys
  methods: {
    create () {

      // eslint-disable-next-line
      console.log('hey');

    }

  },
  mounted () {

    // eslint-disable-next-line
    console.log(this.isadmin);

  },
};
</script>

<style : scoped>
#Home{
    padding: 10px 15px;
  font-size: 10px;
  text-align: center;
  cursor: pointer;
  outline: none;
  color: black;
  background-color: grey;
  border: none;
  border-radius: 10px;
  box-shadow: 0 4px #999;
  float: left;
  margin-left: 10;
  margin-right: 0;
}
#Home:hover {
  cursor: pointer;
  background-color: white
}
#Create_Ticket{
 padding: 10px 15px;
  font-size: 10px;
  text-align: center;
  cursor: pointer;
  outline: none;
  color: black;
  background-color: grey;
  border: none;
  border-radius: 10px;
  box-shadow: 0 4px #999;
  float: left;
margin-left: 10;
margin-right: 0;
}
#Create_Ticket:hover {
  cursor: pointer;
  background-color: white
}
#bar{
    background-color:rgb(196, 191, 192);
    height:35px;
    padding: 5px 5px;
}
img{
  height: 35px;
  width: 35px;
  margin-top: 0;
  float: right;
}
</style>
