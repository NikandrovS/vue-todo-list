<template>
  <nav class="teal darken-3">
    <div class="nav-wrapper">
      <router-link to="/" class="brand-logo">Tasks</router-link>
      <ul class="right hide-on-med-and-down">
        <router-link
            to="/"
            tag="li"
            exact
            active-class="active"
        >Create</router-link>
        <router-link
            to="/list"
            tag="li"
            active-class="active"
        >List</router-link>
      </ul>
    </div>
  </nav>
</template>

<script>
export default {
  name: "Navbar"
}
</script>

<style scoped>
  nav {
    padding: 0 2rem;
  }
  ul {
    display: flex;
  }
</style>