<template>
  <div>
    <header>
      <div
        class="text-center bg-image title-page"
      >
        <div class="mask">
          <div  class="d-flex justify-content-center align-items-center">
            <div  class="text-white">
              <h1  style="margin-top:10%" class="display-1">Accenture coffee</h1>
              <h4 class="mb-3">
                Keeping track of payments made by coffee enthousiasts
              </h4>
            </div>
          </div>
        </div>
      </div>
      <!-- Background image -->
    </header>
    <Customers />
  </div>
</template>

<script>
import Customers from "./components/Customers.vue";

export default {
  name: "App",
  components: {
    Customers,
  },
  data() {
    return {
      orders: [],
      payments: [],
      prices: [],
    };
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.title-page {
  background-image: url("https://images.pexels.com/photos/1141792/pexels-photo-1141792.jpeg?auto=compress&cs=tinysrgb&dpr=3&h=750&w=1260");
  height: 300px;
}

body {
  background: #e2dca6;
}
</style>
