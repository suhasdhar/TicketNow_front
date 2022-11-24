<template>
    <div  >
<table align="center">
<tr v-for="(item,index) in this.ticket" :key="index"> <td> {{index}}</td> <td id="elaborate">{{item}}</td></tr>
</table>
<div>
    Chat here
</div>

    </div>
</template>
<script>

export default {
  mounted () {

    // eslint-disable-next-line
// eslint-disable-next-line
console.log(this.ticket)

  },
  name: 'LoadTicket',


  props: {
    ticket: Object
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
</style>
