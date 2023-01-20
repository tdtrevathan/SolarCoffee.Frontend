<template>
  <div class="side-menu-container">
    <router-link to="/">
      <img
        id="imgLogo"
        alt="Solar Coffee logo"
        src="../assets/images/wendel.jpeg"
      />
    </router-link>
    <h1>Management Portal</h1>
    <solar-button
      id="menuInventory"
      is-full-width
      @button:click="goToRoute('/inventory')"
    >
      Inventory
    </solar-button>
    <solar-button
      id="menuCustomers"
      is-full-width
      @button:click="goToRoute('/customers')"
    >
      Manage Customers
    </solar-button>
    <solar-button
      id="menuInvoice"
      is-full-width
      @button:click="goToRoute('/invoice/new')"
    >
      New Invoice
    </solar-button>
    <solar-button
      id="menuOders"
      is-full-width
      @button:click="goToRoute('/orders')"
    >
      Orders
    </solar-button>
  </div>
</template>

<script lang="ts">
import { Options, Vue } from "vue-class-component";
import SolarButton from "@/components/SolarButton.vue";

@Options({
  name: "SideMenu",
  components: { SolarButton },
})
export default class SideMenu extends Vue {
  goToRoute(route: string) {
    this.$router.push(route);
  }
}
</script>

<style scoped lang="scss">
@import "@/scss/global.scss";
.side-menu-container {
  background-color: #fcfcfc;
  height: 100vh;
  display: flex;
  flex-direction: column;
  padding: 0.8rem;
  box-shadow: 0 1px rgba(0, 0, 0, 12), 0 1px 1px rgba(0, 0, 0, 24);
  box-sizing: border-box;
  width: $menu-width;
}

#imgLogo {
  max-width: 100%;
}

h1 {
  font-size: 1.2rem;
  margin: 1rem 0;
}
</style>
