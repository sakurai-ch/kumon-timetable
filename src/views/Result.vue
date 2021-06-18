<template>
  <div class="list">
    <Header :headerTitle="headerTitle"></Header>

    <div class="main">
      <div class="left">
        <div class="user-list">
          <p class="subtitle">希望者リスト</p>
          <p class="subtitle subtitle-mobile">未確定者</p>
          <UserList :userList="userList"></UserList>
        </div>
        <div class="button-mobile">
          <div>
            <button class="send-button" @click="list">希望者一覧</button>
          </div>
          <div>
            <button class="link-button" @click="home">希望入力画面</button>
          </div>
        </div>
      </div>

      <div class="right">
        <span class="subtitle">時間割</span>
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
      headerTitle: "時間割 集計結果",
      userList: "",
      frames: "",
    };
  },

  methods:{
    home() {
      this.$router.push({ name: "Home" });
    },
    list() {
      this.$router.push({ name: "List" });
    }
  },

  async created(){
    const data = await axios.get("https://kumon-timetable-api.wela.work/api/result");
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

.subtitle-mobile{
  display: none;
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
  width: 130px;
  line-height: 30px;
  cursor: pointer;
  border-radius: 5px;
  font-size: 20px;
  font-weight: bold;
  color: gray;
  background-color: rgb(250, 250, 250);;
  border:3px solid gray;
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
  
  .left .subtitle{
    display: none;
  }
  
  .left .subtitle-mobile{
    display: inline;
  }

  .send-button{
    height: 40px;
    width: 150px;
    margin-left: 0px;
    margin-top: 10px;
    float:left;
  }

  .link-button{
    height: 30px;
    width: 80px;
    margin-left: 0px;
    margin-top: 15px;
    font-size: 8px;
  }

  .button-mobile{
    display: flex;
    justify-content: space-between;
  }

  .right {
    width: 94%;
    padding: 20px;
    position:static;
  }

  .comment{
    font-size: 12px;
  }

  .user-name-input th {
    font-size: 12px;
  }
}

</style> 
