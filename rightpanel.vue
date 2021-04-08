<template>
  <div class="right">
    <h2 class="text-center font-weight-bold">Please Add To-Do List</h2>
    <div class="row" v-if="this.data.todo !== undefined">
      <div class="col-4">
        <div v-if="this.data.todo.length > 0" class="totalTask">
          Total: {{ this.data.todo.length }}
        </div>
      </div>
      <div class="col-4">
        <div v-if="this.data.todo.length > 0" class="completedTaskCount">
          Completed: {{ this.completedCount() }}
        </div>
      </div>
      <div class="col-4">
        <div v-if="this.data.todo.length > 0" class="pendingTaskCount">
          Pending: {{ this.pendingCount() }}
        </div>
      </div>
    </div>
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
      v-if="addTaskInputShow && this.userid != undefined"
      type="text"
      v-on:keyup.enter="addTask($event)"
      class="form-control inputAddTask"
      placeholder="Add Task"
      v-model="value"
    />
    <div v-for="(item, index) in data.todo" :key="index">
      <div class="mb-2">
        <div class="row ">
          <div class="col-7" v-if="editTaskInput && index == edit">
            <input
              type="text"
              v-on:keyup="addTask($event)"
              class="form-control inputEditTask"
              v-if="item.task.length < 20"
              v-model="val"
            />
            <textarea
              v-on:keyup="addTask($event)"
              class="form-control inputEditTask"
              v-else
              v-model="val"
            />
          </div>
          <div v-else class="col-7 todoList">
            <div class="text-white font-weight-semibold p-1">
              <span class="form-check">
                <input
                  type="checkbox"
                  class="form-check-input"
                  :id="`${item.task + index + userid}`"
                  :value="`${item.task + index + userid}`"
                  v-model="checked"
                  v-on:change="getCheckboxIndex(index)"/></span
              ><span
                class="m-4"
                :class="item.isSelected ? 'completedTask' : null"
                >{{ item.task }}</span
              >
            </div>
          </div>

          <div class="col-4" v-if="editTaskInput && index == edit">
            <font-awesome-icon
              :icon="['fas', 'trash']"
              v-on:click="deleteTask(index)"
              class="font-weight-bold mx-2 todoBtn"
            />
          </div>

          <div class="col-4 todoBtn" v-else>
            <font-awesome-icon
              :icon="['fas', 'trash']"
              v-on:click="deleteTask(index)"
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
  props: ["data", "userid"],
  data() {
    return {
      val: "",
      edit: -1,
      addTaskBtnShow: true,
      checked: [],
      addTaskInputShow: false,
      editTaskInput: false,
      value: "",
      pending: "",
      completed: 0,
      viewMore: false,
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
    deleteTask(index) {
      let d = this.checked.find(
        (item) => item === this.data.todo[index].task + 0 + this.userid
      );
      this.checked.splice(d, 1);
      console.log("item", index);
      this.$emit("deletedTask", index);
      // let deleteItem = this.data.todo.filter((n) => n != item);
      // this.data.todo = deleteItem;
    },
    editTask(item) {
      this.val = item.task;
      let itemIndex = this.data.todo.findIndex((n) => n === item);
      this.edit = itemIndex;

      this.editTaskInput = true;
    },
    inputFieldShow() {
      this.addTaskBtnShow = false;
      this.addTaskInputShow = true;
    },
    getCheckboxIndex(index) {
      //this.data.todo[index].isSelected = !this.data.todo[index].isSelected;

      this.$emit("completedTask", index);
      this.completed = this.completedCount();
      this.pending = this.pendingCount();
    },
    completedCount() {
      let count = 0;
      for (let i = 0; i < this.data.todo.length; i++) {
        if (this.data.todo[i].isSelected == true) {
          count = count + 1;
        }
      }
      return count;
    },
    pendingCount() {
      let count = 0;
      for (let i = 0; i < this.data.todo.length; i++) {
        if (this.data.todo[i].isSelected == false) {
          count = count + 1;
        }
      }
      return count;
    },
    addTask(e) {
      this.val = e.currentTarget.value;
      var task = {
        task: e.currentTarget.value,
        isSelected: false,
      };
      if (this.edit < 0) {
        if (e.keyCode === 13) {
          //this.data.todo.push(task);
          this.val = "";

          this.addTaskInputShow = false;
          this.addTaskBtnShow = true;
          this.$emit("addedTask", task);
        }
      } else {
        if (e.keyCode === 13) {
          // this.data.todo[this.edit].task = e.currentTarget.value;
          // this.data.todo[this.edit].isSelected = false;
          let editData = {
            value: e.currentTarget.value,
            index: this.edit,
            isSelected: false,
          };
          this.val = "";
          this.edit = -1;

          this.editTaskInput = false;
          // console.log(this.data);
          this.$emit("editedTask", editData);
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
  word-wrap: break-word;
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
.completedTask {
  text-decoration: line-through;
}
.totalTask {
  margin-left: 10px;
  background: rgb(255, 179, 179);
  width: 60%;
  text-align: center;
  padding: 10px;
  margin: 20px;
  border-radius: 5px;
  box-shadow: 5px 5px 5px #999898;
}
.completedTaskCount {
  margin-left: 10px;
  background: rgb(78, 237, 248);
  width: 60%;
  text-align: center;
  padding: 10px;
  margin: 20px;
  border-radius: 5px;
  box-shadow: 5px 5px 5px #999898;
}
.pendingTaskCount {
  margin-left: 10px;
  background: rgb(223, 248, 78);
  width: 60%;
  text-align: center;
  padding: 10px;
  margin: 20px;
  border-radius: 5px;
  box-shadow: 5px 5px 5px #999898;
}
</style>
