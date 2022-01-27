<template>
  <div id="app">

    <div>

        <div> {{forum.user_id}}</div>
        <input type="text" v-model="forum.id" /> <br />
        <input type="text" v-model="forum.title" /> <br />
        <input type="text" v-model="forum.content" id="contents"/> <br />

    </div>

    <div>
    <button @click="createforum">글쓰기</button>
    <button @click="deleteforum">삭제</button>
    <button @click="updateforum">수정</button>

      <div>
        <button @click="readforum">글 읽어오기</button>

        <!--      {{ items }}-->
        <div v-for="forum in forums" :key="forum.id" :item="forum">
          {{ forum.user_id }} || {{ forum.title }} || {{ forum.content }}
        </div>

      </div>

    </div>


  </div>
</template>



<script>
import axios from 'axios'

const URL_forum = 'http://localhost:8000/api/forums'

export default {
  name: 'App',

  data() {
    return {
      forum: {

        id: 0,         //글 번호
        user_id:'',    //유저 아이디
        title:'',      // 제목
        content:'',    // 글내용

        },

      forumId: 0,
      forums: [],     // 글 전체

    }
  },


  methods: {
    async createforum() {
      const res = await axios.post(URL_forum, { ...this.forum })  //forum 전체를 post 한다는 의미.
      console.log(res)
    },

    async deleteforum() {
      const res = await axios.delete(URL_forum + '/' + this.forumId)
      console.log(res.data)
    },

    async readforum() {
      const res = await axios.get(URL_forum)
      this.items = res.data
      console.log(res)
    },

    async updateforum() {
      const res = await axios.put(URL_forum + '/' + this.forumId, { ...this.forum })  //(이 주소에, 이 내용을 넣는다.)
      console.log(res)
    },
  },



  watch: {
    'forum': {
      deep: true,
      handler() {
        console.log('watch', this.forum)
      },
    },
  },
}
</script>






<style>

input{
  width: 350px;
  height: 30px;
  margin-top: 5px;
  margin-bottom: 5px;
}

input#contents {

  height: 500px;

}


</style>
