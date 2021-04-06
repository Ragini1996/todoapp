<template>
  <div>
    <Navbar :data="data" />
    <div class="row">
      <div class="col-lg-3 col-md-3 col-sm-6 col-xs-12 ">
        <LeftPanel v-on:viewToDo="view($event)" :users="users" />
      </div>
      <div class="col-lg-8 col-md-8 col-sm-6 col-xs-12 background">
        <RightPanel
          :data="data"
          v-on:addedTask="getTask($event)"
          :value="value"
        />
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
    };
  },
  methods: {
    view(value) {
      this.data = { name: value.name, todo: value.todo };
      this.value = value.newText;
    },
    /* getTask(value) {
      let ind = this.users.findIndex((n) => n.name === value.name);
      console.log("ind", this.users[ind]);
      this.users[ind] = value;
    },*/
  },
};
</script>
<style>
.background {
  background: linear-gradient(rgba(0, 0, 0, 0.3), rgba(0, 0, 0, 0.3)),
    url("../assets/images.jpg");
  background-attachment: fixed;
  position: relative;
  background-position: center;
  height: 450px;
  background-size: cover;
}
</style>
