<template>
  <header  role="banner">
    <b-navbar toggleable="lg" type="light" variant="light" class>
      <b-navbar-brand href="#">Mi Blog</b-navbar-brand>

      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

      <b-collapse id="nav-collapse" is-nav>
        <b-navbar-nav>
          <b-nav-item href="#" class="myclass">Link</b-nav-item>
        </b-navbar-nav>

        <!-- Right aligned nav items -->
        <b-navbar-nav class="ml-auto">
          <b-nav-form class="myclass">
            <b-nav-item href="#">Home</b-nav-item>
          </b-nav-form>
          <b-nav-form class="myclass">
            <b-nav-item href="#">Home</b-nav-item>
          </b-nav-form>
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>
  </header>
</template>

<script>
export default {}
</script>

<style>
</style>