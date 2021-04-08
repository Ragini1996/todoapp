<template>
  <div>
    <Navbar :data="data" />
    <div class="container-fluid background">
      <div class="row">
        <LeftPanel
          v-on:viewToDo="view($event)"
          :users="users"
          v-on:usersAdd="usersAdded($event)"
          class="col-lg-3 col-md-3 col-sm-4 col-xs-12 "
        />
        <div class="col-lg-8 col-md-9 col-sm-8 col-xs-12 ">
          <RightPanel
            :data="data"
            v-on:addedTask="getTask($event)"
            v-on:editedTask="editTask($event)"
            v-on:deletedTask="deleteTask($event)"
            v-on:completedTask="completedTask($event)"
            :value="value"
            :userid="userid"
          />
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import Navbar from "./navbar.vue";
import LeftPanel from "./leftpanel.vue";
import RightPanel from "./rightpanel.vue";
export default {
  name: "Body",
  components: {
    Navbar,
    LeftPanel,
    RightPanel,
  },
  data() {
    return {
      users: [
        { name: "Mohit", todo: [] },
        { name: "Kirti", todo: [] },
        { name: "Jyotsana", todo: [] },
        { name: "Ravi", todo: [] },
      ],
      data: {},
      value: "",
      userid: -1,
    };
  },
  methods: {
    view(value) {
      let index = this.users.findIndex((n) => n.name === value.name);
      this.data = { name: value.name, todo: value.todo };
      this.value = value.newText;
      this.userid = index;
    },
    getTask(value) {
      let itemIndex = this.users.findIndex((n) => n.name === this.data.name);
      this.users[itemIndex].todo.push(value);
    },
    editTask(value) {
      let itemIndex = this.users.findIndex((n) => n.name === this.data.name);
      this.users[itemIndex].todo[value.index].task = value.value;
      this.users[itemIndex].todo[value.index].isSelected = value.isSelected;
    },
    deleteTask(index) {
      let itemIndex = this.users.findIndex((n) => n.name === this.data.name);
      this.users[itemIndex].todo.splice(index, 1);
    },
    completedTask(index) {
      let itemIndex = this.users.findIndex((n) => n.name === this.data.name);
      this.users[itemIndex].todo[index].isSelected = !this.users[itemIndex]
        .todo[index].isSelected;
      //console.log(this.users);
    },
    usersAdded(users) {
      let userData = { name: users, todo: [] };
      this.users.push(userData);
    },
  },
};
</script>
<style>
body {
  background: linear-gradient(rgba(0, 0, 0, 0.3), rgba(0, 0, 0, 0.3)),
    url("../assets/images.jpg");
  background-attachment: fixed;
  position: relative;
  background-position: center;
  background-size: cover;
}
</style>
