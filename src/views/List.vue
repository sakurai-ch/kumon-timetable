<template>
  <div class="list">
    <Header :headerTitle="headerTitle"></Header>

    <div class="main">
      <div class="left">
        <div class="user-list">
          <p class="subtitle">希望者リスト</p>
          <UserList :userList="userList"></UserList>
        </div>
        <div>
          <button class="send-button" @click="result">時間割 集計</button>
        </div>
        <div>
          <button class="link-button" @click="home">希望入力画面</button>
        </div>
      </div>

      <div class="right">
        <span class="subtitle">希望時間一覧</span>
        <TimeTable :frames="frames"></TimeTable>
      </div>
    </div>

  </div>
</template>

<script>
import Header from "../components/Header";
import UserList from "../components/UserList";
import TimeTable from "../components/TimeTable";
import axios from "axios";
export default {
  name: 'Home',
  components: {
    Header,
    UserList,
    TimeTable,
  },
  data() {
    return {
      headerTitle: "面談希望 一覧",
      userList: "",
      frames: "",
    };
  },

  methods:{
    home() {
      this.$router.push({ name: "Home" });
    },
    result() {
      this.$router.push({ name: "Result" });
    }
  },

  async created(){
    const data = await axios.get("http://127.0.0.1:8000/api/list");
    this.frames = data.data.frames_data;
    this.$store.state.dates = data.data.date_data;
    this.userList = data.data.user_list_data;
  }
}
</script>

<style scoped>
.main {
  display: flex;
}

.subtitle{
  font-size: 20px;
  font-weight: bold;
  color: rgb(126, 126, 126);
}

.comment{
  font-size: 14px;
  color: rgb(126, 126, 126);
  margin-left: 20px;
}

.right {
  width: 70%;
  padding: 20px;
  position: absolute;
  left: 250px;
}

.left {
  padding: 20px;
}

.send-button{
  width: 150px;
  line-height: 30px;
  cursor: pointer;
  border-radius: 5px;
  font-size: 20px;
  font-weight: bold;
  color: rgb(110, 209, 255);
  background-color: rgb(213, 255, 255);
  border:3px solid aqua;
  margin-top:20px;
}

.link-button{
  width: 100px;
  cursor: pointer;
  border-radius: 5px;
  color: gray;
  background-color: white;
  border-color:rgb(173, 173, 173);
  margin-top:20px;
}


@media screen and (max-width : 480px){
  .main {
    display:initial;
  }

  .left{
    display: flex;
    justify-content: space-between;
  }
  
  .left .user-list{
    display: none;
  }

  .send-button{
    height: 50px;
    width: 150px;
    margin-left: 0px;
    margin-top: 5px;
  }

  .link-button{
    height: 30px;
    width: 100px;
    margin-left: 50px;
    margin-top: 15px;
    font-size: 8px;
  }

  .right {
    width: 94%;
    padding: 20px;
    position:static;
  }

  .comment{
    font-size: 12px;
  }
}

</style> 
