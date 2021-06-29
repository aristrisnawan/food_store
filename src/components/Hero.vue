<template>
  <div class="hero">
    <!-- Dekstop -->
    <div class="d-none d-md-block">
      <div class="row">
        <div class="col-md-6">
          <div class="d-flex h-100">
            <div class="justify-content-center align-self-center">
              <h2>
                <strong>Delicious Food Menu, </strong> <br />in your gedget
              </h2>
              <p>Come on, choose your favorite food !!!</p>
              <router-link to="/foods" class="btn btn-lg btn-success">
                <b-icon-arrow-right></b-icon-arrow-right> Order Now
              </router-link>
            </div>
          </div>
        </div>
        <div class="col-md-6">
          <img src="../assets/image/hero.png" width="100%" alt="" />
        </div>
      </div>
    </div>

    <!-- Mobile -->
    <div class="d-sm-block d-md-none">
      <div class="row mt-4">
        <div class="col-md-6">
          <img src="../assets/image/hero.png" width="100%" alt=""/>
        </div>
        <div class="col-md-6">
          <div class="d-flex h-100 mt-4">
            <div class="justify-content-center align-self-center">
              <h2>
                <strong>Delicious Food Menu, </strong> <br />in your gedget
              </h2>
              <p>Come on, choose your favorite food !!!</p>
              <router-link to="/foods" class="btn btn-md btn-success">
                <b-icon-arrow-right></b-icon-arrow-right> Order Now
              </router-link>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Hero",
};
</script>

<style>
</style>