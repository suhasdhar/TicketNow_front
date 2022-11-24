<!-- eslint-disable sort-keys -->
<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p>
      Your one stop solution for Problem managemment
    </p>
  </div>
</template>

<script>

export default {
  // Mounted () {

  /*
   *   // eslint-disable-next-line
   *   fetch('http://localhost:8080/api/auth/signin',{
   *   // eslint-disable-next-line sort-keys
   *     headers: {
   *       'Authorization': 'Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJzeGEzMDU5MCIsImlhdCI6MTY2OTEzOTQ1NCwiZXhwIjoxNjY5MjI1ODU0fQ.osuw523cHOUpG7cFGRqZrfcGumzGI3w_NmLDD1gt-wJCa79piIWRm8Wc7qcZ1duA0b0-95SbwtIMNLGyucIYrw',
   *       // eslint-disable-next-line sort-keys
   *       'Accept': 'application/json',
   *       'Content-Type': 'application/json'
   *     },
   *     method: 'GET'}).then((response) => response.json()).
   *     then((text) => {
   */

  /*
   *       // eslint-disable-next-line
   *       console.log(text);
   */

  /*
   *     }).
   *     catch((error) => {
   */

  /*
   *       // eslint-disable-next-line
   *       console.log(error);
   *       // eslint-disable-next-line
   *       console.log('fail ho gya bhai ');
   */

  //     });

  // },
  name: 'HelloWorld',
  props: {
    msg: String,
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
