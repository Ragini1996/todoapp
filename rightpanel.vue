<template>
  <div class="right">
    <h2 class="text-center font-weight-bold">Please Add To-Do List</h2>
    <div v-if="data.name">
      <button
        class="btn btn-danger"
        v-if="addTaskBtnShow"
        v-on:click="inputFieldShow"
      >
        Add Task
      </button>
    </div>
    <div v-else></div>

    <input
      v-if="addTaskInputShow"
      type="text"
      v-on:keyup.enter="addTask($event)"
      class="form-control inputAddTask"
      placeholder="Add Task"
      v-model="value"
    />
    <div v-for="(item, index) in data.todo" :key="item">
      <div class="mb-2">
        <div class="row ">
          <div class="col-5" v-if="editTaskInput && index == edit">
            <input
              type="text"
              v-on:keyup="addTask($event)"
              class="form-control inputEditTask"
              v-model="val"
            />
          </div>
          <div v-else class="col-5 todoList">
            <div class="text-white font-weight-semibold p-1">{{ item }}</div>
          </div>
          <div class="col-5" v-if="editTaskInput && index == edit"></div>
          <div class="col-5 todoBtn" v-else>
            <font-awesome-icon
              :icon="['fas', 'trash']"
              v-on:click="deleteTask(item)"
              class="font-weight-bold mx-2"
            />
            <font-awesome-icon
              :icon="['fas', 'edit']"
              v-on:click="editTask(item)"
              class="font-weight-bold mx-2"
            />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "RightPanel",
  props: ["data"],
  data() {
    return {
      val: "",
      edit: -1,
      addTaskBtnShow: true,
      addTaskInputShow: false,
      editTaskInput: false,
      value: "",
    };
  },
  created() {
    this.$root.$on("updateInputField", () => {
      if (this.addTaskInputShow === true) {
        this.addTaskBtnShow = !this.addTaskBtnShow;
        this.addTaskInputShow = !this.addTaskInputShow;
      } else {
        this.addTaskBtnShow = true;
        this.addTaskInputShow = false;
      }
      this.value = "";
    });
  },
  methods: {
    deleteTask(item) {
      let itemIndex = this.data.todo.findIndex((n) => n === item);
      this.data.todo.splice(itemIndex, 1);
      // let deleteItem = this.data.todo.filter((n) => n != item);
      // this.data.todo = deleteItem;
    },
    editTask(item) {
      this.val = item;
      let itemIndex = this.data.todo.findIndex((n) => n === item);
      this.edit = itemIndex;

      this.editTaskInput = true;
    },
    inputFieldShow() {
      this.addTaskBtnShow = false;
      this.addTaskInputShow = true;
    },
    addTask(e) {
      this.val = e.currentTarget.value;
      if (this.edit < 0) {
        if (e.keyCode === 13) {
          this.data.todo.push(e.currentTarget.value);
          this.val = "";

          this.addTaskInputShow = false;
          this.addTaskBtnShow = true;
          //this.$emit("addedTask", this.data);
        }
      } else {
        if (e.keyCode === 13) {
          this.data.todo[this.edit] = e.currentTarget.value;
          this.val = "";
          this.edit = -1;

          this.editTaskInput = false;
          console.log(this.data);
          //this.$emit("addedTask", this.data);
        }
      }
      this.value = "";
    },
  },
};
</script>
<style scoped>
.todoList {
  margin-top: 20px;
  background: rgba(248, 248, 246, 0.3);
  border-radius: 5px;
  margin-left: 15px;
}
.todoBtn {
  margin-left: 30px;
  margin-top: 25px;
}
.inputAddTask {
  width: 80%;
}
.inputEditTask {
  margin-top: 20px;
}
</style>
