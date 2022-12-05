<template>
<div>
    <br><br>
    <div class="comment-box">

        <div class="comment-thread" v-for="item in this.comments" :key="item">
    <!-- Comment 1 start -->
    <div class="comment" id="comment-1">
        <div class="comment-heading">
            <div class="comment-voting">
                <button type="button">
                    <span aria-hidden="true">&#9650;</span>
                    <span class="sr-only">Vote up</span>
                </button>
                <button type="button">
                    <span aria-hidden="true">&#9660;</span>
                    <span class="sr-only">Vote down</span>
                </button>
            </div>
            <div class="comment-info">
                <a href="#" class="comment-author">{{item.username}}</a>
            </div>
        </div>

        <div class="comment-body">
            <p>
               {{item.comment}}
            </p>
        </div>

    </div>
    <!-- Comment 1 end -->
    <hr>
    </div>

</div>

<br><button @click="this.fetchChat()"> Refresh Comments</button><br> <br>
    <textarea
            cols="50"
            rows="10"
            id:="comment-textbox"
            required
            placeholder="Add your comment"
            v-model="commentsnew"
          />
          <br><br>
          <button @click="addcomment()"> Add Comment</button>
          <div v-if="this.nocommenterrot" class="error"> The comment cant be empty</div>

</div>

</template>

<script>
export default {data () {

  return {

    comments: {},
    commentsnew: null,
    nocommenterrot: false


  };

},
methods: {
  async addcomment () {

    if (this.commentsnew === null) {

      this.nocommenterrot = true;
      return;

    }
    await fetch(
      'http://localhost:8080/ticket-now/add-message',
      {body: JSON.stringify({'message': {
        'comment': this.commentsnew,
        'userId': this.id,
        'username': this.id,
      },
      'ticketId': this.ticketId}),
      headers: {'Accept': 'application/json, text/plain',
        'Content-Type': 'application/json;charset=UTF-8',
        'ticketId': this.ticketId},
      method: 'POST',
      mode: 'cors'}
    ).then((res) => res.json()).
      then((data) => {

        // eslint-disable-next-line
                console.log(data);
      }).
      // eslint-disable-next-line
        catch((error) => console.log(error));
    this.fetchChat();
    this.commentsnew = null;

  },

  async fetchChat () {

    // eslint-disable-next-line
console.log(this.ticket)

    await fetch(
      'http://localhost:8080/ticket-now/get-chats',
      {headers: {'Accept': 'application/json, text/plain',
        'Content-Type': 'application/json;charset=UTF-8',
        'ticketId': this.ticketId},
      method: 'get',
      mode: 'cors'}
    ).then((res) => res.json()).
      then((data) => {

        this.comments = data.message;
        // eslint-disable-next-line
      
      }).
      // eslint-disable-next-line
        catch((error) => console.log(error));
  },

},

mounted () {

  // eslint-disable-next-line
  console.log(this.id)+" somethis???";
  this.fetchChat();

},
name: 'CommentsChat',
props: {

  id: String,
  ticket: Object,
  ticketId: String

}};


</script>

<style : scoped>

* {
    box-sizing: border-box;
}
body {
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";
    line-height: 1.4;
    color: rgba(0, 0, 0, 0.85);
    background-color: #f9f9f9;

}
button {
    -moz-appearance: none;
    -webkit-appearance: none;
    appearance: none;
    font-size: 14px;
    padding: 4px 8px;
    color: rgba(0, 0, 0, 0.85);
    background-color: #fff;
    border: 1px solid rgba(0, 0, 0, 0.2);
    border-radius: 4px;
}
button:hover,
button:focus,
button:active {
    cursor: pointer;
    background-color: #ecf0f1;
}
.comment-thread {
    width: 700px;
    max-width: 100%;
    margin: auto;
    padding: 0 30px;
    background-color: #fff;
    border: 1px solid transparent; /* Removes margin collapse */
}
.m-0 {
    margin: 0;
}
.sr-only {
    position: absolute;
    left: -10000px;
    top: auto;
    width: 1px;
    height: 1px;
    overflow: hidden;
}

/* Comment */

.comment {
    position: relative;
    margin: 20px auto;
}
.comment-heading {
    display: flex;
    align-items: center;
    height: 50px;
    font-size: 14px;
}
.comment-voting {
    width: 20px;
    height: 32px;
    border: 1px solid rgba(0, 0, 0, 0.2);
    border-radius: 4px;
}
.comment-voting button {
    display: block;
    width: 100%;
    height: 50%;
    padding: 0;
    border: 0;
    font-size: 10px;
}
.comment-info {
    color: rgba(0, 0, 0, 0.5);
    margin-left: 10px;
}
.comment-author {
    color: rgba(0, 0, 0, 0.85);
    font-weight: bold;
    text-decoration: none;
}
.comment-author:hover {
    text-decoration: underline;
}
.replies {
    margin-left: 20px;
}

.error{

    color: red;
}
.comment-thread{
    width: 50%;
    height: 120px;
    -moz-border-radius: 100px / 50px;
    -webkit-border-radius: 100px / 178px;
    border-radius: 30px / 20px;

    z-index: 100;
    position: relative;
    background-color: rgb(196, 188, 188);
}
</style>
