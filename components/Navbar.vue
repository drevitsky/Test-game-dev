<template>
  <div>
  <b-navbar toggleable="lg" type="dark" variant="info">
    <b-navbar-brand href="#">Test game for future employer</b-navbar-brand>

    <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

    <b-collapse id="nav-collapse" is-nav>
      <b-navbar-nav>
        <nuxt-link class="my-link" exact active-class="active" to="/">Game</nuxt-link>
        <nuxt-link class="my-link" active-class="active" to="/my">My CV</nuxt-link>
      </b-navbar-nav>

    </b-collapse>
  </b-navbar>
  <div class="navbar">
      <ul class="menu-list">

      </ul>
  </div>
</div>
</template>

<script>
export default {

}
</script>

<style>
.my-link {
  margin: 20px;
  color: #fff;
  font-size: 30px;
  font-weight: 600;
  transition: all 0.6s;
}
.my-link:hover {
  color: #5558;
  text-decoration: none;
}
.active {
  color: #555;
}

</style>