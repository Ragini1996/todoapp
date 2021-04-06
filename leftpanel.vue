<template>
  <div>
    <ul
      v-for="user in users"
      :key="user.name"
      class="list-group text-center my-1 "
    >
      <li
        v-on:click="getToDo(user.name)"
        :class="
          name === user.name
            ? 'list-group-item list-group-item-action list-group-item-danger'
            : 'list-group-item list-group-item-action '
        "
        id="list-profile-list"
        data-toggle="list"
        href="#list-profile"
        role="tab"
      >
        {{ user.name }}
      </li>
    </ul>
    <button
      class="btn btn-primary btn-block "
      v-on:click="showInput"
      v-if="addBtnShow"
    >
      Add New
    </button>
    <div v-if="show">
      <input
        type="text"
        v-on:keyup="addToUsers($event)"
        id="name"
        class="form-control"
        placeholder="Enter name"
      />
    </div>
  </div>
</template>
<script>
export default {
  name: "LeftPanel",
  props: ["users"],
  data() {
    return { show: false, name: "", addBtnShow: true, newText: "" };
  },
  methods: {
    getToDo(name) {
      let value = this.users.find((n) => n.name === name);
      this.name = name;
      this.$root.$emit("updateInputField");
      this.$emit("viewToDo", value, this.newText);
    },
    showInput() {
      this.show = true;
      this.addBtnShow = false;
    },
    addToUsers(e) {
      if (e.keyCode === 13) {
        this.users.push({ name: e.currentTarget.value, todo: [] });
        this.show = false;
        this.addBtnShow = true;
      }
    },
  },
};
</script>
<style></style>
