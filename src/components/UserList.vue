<template>
  <div>
    <table class="user-list">
      <tr>
        <th>名前</th><th>希望<br>時間数</th><th>確定<br>状況</th>
      </tr>
      <tr v-for="(item, index) in userList" :key="index">
        <td>
          <div class="box" @click="checkUser(item)" :class="item.state">
            <div class="content-box">
              <p>{{item.name}}</p>
            </div>
          </div>
        </td>
        <td>
          {{item.quantity}}
        </td>
        <td v-if="item.state === 'fixed'">
          済
        </td>
        <td v-else-if="item.state === 'warning'">
          <p class="red">未</p>
        </td>
        <td v-else>
          未
        </td>
      </tr>
    </table>

    <table class="user-list-mobail">
      <tr>
        <th>名前</th><th>希望<br>時間数</th><th>確定<br>状況</th>
      </tr>
      <tr v-for="(item, index) in userList" :key="index">
        <td v-if="item.state === 'warning'">
          <div class="box" @click="checkUser(item)" :class="item.state">
            <div class="content-box">
              <p>{{item.name}}</p>
            </div>
          </div>
        </td>
        <td v-if="item.state === 'warning'">
          {{item.quantity}}
        </td>
        <td v-if="item.state === 'warning'">
          <p class="red">未</p>
        </td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  props: ["userList"],
  data() {
    return {

    };
  },
  methods: {
    checkUser(item){
      this.userList.forEach(element => {
        element.state = "selectable";
      })

      if(this.$store.state.selectedUser == item.name){
        this.$store.state.selectedUser = "";
      }else{
        this.$store.state.selectedUser = item.name;
        item.state = "selected";
      }
    },
  }
}
</script>

<style scoped>

.user-list,
.user-list-mobail {
  margin-top: 5px;
  width: 210px;
  border: 3px solid gray;
  border-collapse: separate;
  border-radius: 10px;
}

.user-list-mobail{
  display: none;
}

.user-list th,
.user-list td,
.user-list-mobail th,
.user-list-mobail td {
  text-align: center;
  vertical-align: middle;
  border: 1px solid gray;
  border-collapse: separate;
  padding: 2px;
  cursor: default;
  color: rgb(90, 90, 90);
}

.user-list th,
.user-list-mobail th {
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

.select-box {
  cursor: pointer;
}

.content-box {
  margin-top: 5px;
  margin-bottom: 5px;
  font-size: 14px;
}

.close{
  border-color: gray;
  background-color: rgb(247, 247, 247);
  color: rgb(209, 209, 209);
}

.selected {
  border: 2px solid rgb(255, 22, 100);
  background-color: rgb(247, 247, 247);
  color: rgb(255, 22, 100);
  cursor: pointer;
}

.selectable {
  border-color: aqua;
  color: gray;
  cursor: pointer;
}

.unselectable{
  border-color: gray;
  color: gray;
}

.fixed{
  border-color: white;
  color: gray;
}

.warning {
  border: 2px solid rgb(255, 22, 100);
  background-color: yellow;
  color: rgb(255, 22, 100);
}

.red{
  color: rgb(255, 22, 100);
}


@media screen and (max-width : 480px){
  .user-list {
    display: none;
  }
  .user-list-mobail{
    width: 100%;
    display: table;
  }
  .user-list-mobail th {
    height: 30px;
  }
}


</style>