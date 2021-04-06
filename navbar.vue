<template>
  <nav class="navbar navbar-expand-lg navbar-light bg-info">
    <a class="navbar-brand font-weight-bold">To-Do App</a>
    <button
      class="navbar-toggler"
      type="button"
      data-toggle="collapse"
      data-target="#navbarNav"
      aria-controls="navbarNav"
      aria-expanded="false"
      aria-label="Toggle navigation"
    >
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarNav">
      <div class="ml-auto font-weight-bold">{{ data.name }}</div>
    </div>
  </nav>
</template>
<script>
export default {
  name: "Navbar",
  props: ["data"],
};
</script>
<style scoped></style>
