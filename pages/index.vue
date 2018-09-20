<template>
<section class="container">
  <div class="main-innerContainer">
    <app-logo/>
    <h1 class="title">
        todoapp-vue
    </h1>
    <TodoList/>
  </div>
</section>
</template>

<script>
import AppLogo from "~/components/AppLogo.vue";
import TodoList from "~/components/TodoList.vue";

export default {
  components: {
    AppLogo,
    TodoList
  }
};
</script>

<style>
body {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue",
    Arial, sans-serif;
}

.container {
  /* min-height: 40vh; */
  margin-top: 30px;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.main-innerContainer {
  width: 45%;
}

.title {
  /* font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue",
  Arial, sans-serif; */
  /* 1 */
  margin: 20px;
  display: block;
  font-weight: 300;
  font-size: 20px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
