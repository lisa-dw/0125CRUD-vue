<template>
  <div id="app">

    <div>

        <input type="text" v-model="forum.title" /> <br />
        <input type="text" v-model="forum.contents" id="contents"/> <br />

    </div>

    <div>

    <button @click="createforum">글쓰기</button>
    <button @click="updateforum">수정</button> <br />

    <input type="text" v-model="forumId" />
    <button @click="deleteforum">삭제</button>




      <div>
        <button @click="readforums">글 읽어오기</button>

        <!--      {{ items }}-->
        <div v-for="forum in forums" :key="forum.id" :item="forum">
          {{ forum.id }} || {{ forum.title }} || {{ forum.contents }}
        </div>

      </div>

    </div>


  </div>
</template>



<script>
import axios from 'axios'

const URL_forum = 'http://localhost:8001/api/forums'

export default {
  name: 'App',

  data() {
    return {
      forum: {

        id: 0,         //글 번호
        user_id:'',    //유저 아이디
        title:'',      // 제목
        contents:'',    // 글내용

        },

      forumId: 0,
      forums: [],     // 글 전체 -> 배열상태 -> <template>에서 포문을 돌려서 요소 하나하나를 꺼내줌.

    }
  },


  methods: {
    async createforum() {
      const res = await axios.post(URL_forum, { ...this.forum })  //forum 전체를 post 한다는 의미.
      console.log(res)
      console.log(res.data)
      this.forums.push(res.data)  //forums라는 배열의 맨 뒤에 res.data를 바로 집어넣어라
                          // 브라우저에 출력 되어있는 forums에 res.data를 바로 push해서, 브라우저에서 보여준다.


      //await this.readforums()
    },

    async deleteforum() {
      const res = await axios.delete(URL_forum + '/' + this.forumId)
      console.log(res)

      await this.readforums()   //  삭제 후에 바로 readforums 함수를 실행시켜, 브라우저 화면에
    },

    async readforums() {
      const res = await axios.get(URL_forum)
      this.forums = res.data  // this.forums 의 forums 가 data(): 의 forums 이고, 이 forums는 배열[] 이므로
      console.log(res)
    },

    async updateforum() {
      const res = await axios.put(URL_forum + '/' + this.forumId, { ...this.forum })  //(이 주소에, 이 내용을 넣는다.)
      console.log(res)
    },
  },



  // watch: {
  //   'forum': {
  //     deep: true,
  //     handler() {
  //       console.log('watch', this.forum)
  //     },
  //   },
  // },
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
