<template>
  <div class="home">
    <Header :headerTitle="headerTitle"></Header>

    <div class="main">
      <div class="left">
        <div>
          <p class="subtitle">名前</p>
          <table class="user-name-input">
            <tr>
              <th>
                <input type="text" @change="onInput" v-model="$store.state.userName" placeholder="氏名を入力してください" class="box selectable" :class="select">
              </th>
            </tr>
          </table>
        </div>
        <div class="button-mobile">
          <div>
            <button class="send-button" @click="sendRequest">送信</button>
          </div>
          <div>
            <button class="link-button" @click="list">管理用</button>
          </div>
        </div>
      </div>

      <div class="right">
        <span class="subtitle">希望時間</span>
        <span class="comment">※可能な日時を複数選択してください</span>
        <TimeTable :frames="frames"></TimeTable>
      </div>

    </div>

  </div>
</template>

<script>
import Header from "../components/Header";
import TimeTable from "../components/TimeTable";
import axios from "axios";

export default {
  name: 'Home',
  components: {
    Header,
    TimeTable,
  },
  data() {
    return {
      headerTitle: "面談希望 入力フォーム",
      frames: "",
      select: "",
    };
  },

  methods: {
    async sendRequest(){
      if(this.$store.state.userName == ""){
        alert("名前を入力してください");
      }else if(this.$store.state.selectFrame == 0){
        alert("希望時間を選択してください");
      }else{
        await axios.post("http://127.0.0.1:8000/api", {
          userName : this.$store.state.userName,
          timeRequests : this.$store.state.timeRequests,
        });
        alert("希望時間を送信しました");
        this.$router.go({
          path: this.$router.currentRoute.path,
        });
      }
    },

    list() {
      this.$router.push({ name: "List" });
    },

    onInput(){
      if(this.$store.state.userName == ""){
        this.select = "";
      }else{
        this.select = "selected";
      }
    }

  },
  
  async created(){
    const data = await axios.get("http://127.0.0.1:8000/api");
    this.$store.state.timeRequests = data.data.timeRequest_data;
    this.frames = data.data.frames_data;
    this.$store.state.dates = data.data.date_data;
    this.$store.state.userName = "";
  }
}
</script>

<style scoped>
.home .main {
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

.user-name-input {
  margin-top: 5px;
  width: 210px;
  border: 3px solid gray;
  border-collapse: separate;
  border-radius: 10px;
}

.user-name-input th,
.user-name-input td {
  text-align: center;
  vertical-align: middle;
  border: 1px solid gray;
  border-collapse: separate;
  padding: 2px;
  cursor: default;
  color: rgb(90, 90, 90);
}

.user-name-input th {
  height: 50px;
  font-size: 14px;
}

.box {
  height: 80%;
  width: 86%;
  margin: 0 auto;
  border-radius: 5px;
  border: 2px solid;
}

.selectable {
  border-color: aqua;
  color: gray;
}

.selected {
  border: 2px solid rgb(255, 22, 100);
}

.send-button{
  width: 100px;
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

.send-button:hover{
  border-color: rgb(255, 22, 100);
  background-color: rgb(247, 247, 247);
  color: rgb(255, 22, 100);
}

.link-button{
  width: 100px;
  cursor: pointer;
  border-radius: 5px;
  color: gray;
  background-color: white;
  border-color:rgb(173, 173, 173);
  margin-top:100px;
}


@media screen and (max-width : 480px){
  .home .main {
    display:initial;
  }

  .left{
    display: flex;
    justify-content: space-between;
  }

  .send-button{
    height: 50px;
    width: 80px;
    margin-left: 0px;
    margin-top: 30px;
    margin-right: 20px;
  }

  .link-button{
    height: 50px;
    width: 35px;
    margin-left: 0px;
    margin-top: 30px;
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

  .user-name-input {
    width: 180px;
  }

}

</style>
