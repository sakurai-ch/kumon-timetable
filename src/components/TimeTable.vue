
<template>
  <div>
    <table class="timetable">
      <tr>
        <th v-for="(field,index) in $store.state.dates" :key="index">
          <p>{{field.date}}</p>
          <p>{{field.day}}</p>
        </th>
      </tr>

      <tr v-for="(row,index) in frames" :key="index">
        <td v-for="(field,index) in row.frames_row" :key="index">
          <div @click="userClick(field)" class="box" :class="field.state">
            <div class="content-box">
              <span>{{row.time}}</span><span class="mobile">:00</span>
              <div v-for="(item,index) in field.content" :key="index">
                <p v-if="item === $store.state.selectedUser" class="selectedUser">
                  {{item}}
                </p>
                <p v-else>
                  {{item}}
                </p>
              </div>
            </div>
          </div>

        </td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  props: ["frames"],
  methods: {
    userClick(field) {
      if(field.state == "selectable"){
        field.state = "selected";
        this.$store.state.timeRequests[field.id] = true;
        this.$store.state.selectFrame += 1;
      }else if(field.state == "selected"){
        field.state = "selectable";
        this.$store.state.timeRequests[field.id] = false;
        this.$store.state.selectFrame -= 1;
      }

    },
  },
}
</script>

<style scoped>
.subtitle{
  font-size: 20px;
  font-weight: bold;  color: rgb(126, 126, 126);
}

.timetable {
  margin-top: 5px;
  width: 95%;
  border: 3px solid gray;
  border-collapse: separate;
  border-radius: 10px;
  table-layout:fixed;
}

.timetable th,
.timetable td {
  text-align: center;
  vertical-align: middle;
  border: 1px solid gray;
  border-collapse: separate;
  padding: 2px;
  cursor: default
}

.timetable th {
  height: 50px;
  font-size: 14px;
  color: rgb(90, 90, 90);  
}

.box {
  height: 80%;
  width: 86%;
  margin: 0 auto;
  border-radius: 5px;
  border: 2px solid;
}

.content-box {
  margin-top: 10px;
  margin-bottom: 10px;
  font-size: 12px;
}

.close{
  border-color: gray;
  background-color: rgb(250, 250, 250);
  color: rgb(250, 250, 250);
}

.selected {
  border: 2px solid rgb(255, 22, 100);
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

.selectedUser{
  color: rgb(255, 22, 100);
}

@media screen and (max-width : 480px){
  .timetable th {
    font-size: 8px;
  }

  .content-box p{
    font-size: 8px;
  }

  .mobile{
    display: none;
  }

}
</style>