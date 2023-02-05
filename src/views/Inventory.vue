<template>
  <div class="inventory-container">
    <h1 id="inventoryTitle">Inventory Dashboard</h1>
    <hr />
    <div class="inventory-actions">
      <solar-button @click.native="showNewProductModal" id="addNewBtn">
        Add New Item
      </solar-button>
      <solar-button @click.native="showShipmentModal" id="recieveShipmentBtn">
        Reveive Shipment
      </solar-button>
    </div>

    <table id="inventoryTable" Class="table">
      <tr>
        <th>Item</th>
        <th>Quanitity On-hand</th>
        <th>Unit Price</th>
        <th>Taxable</th>
        <th>Delete</th>
      </tr>

      <tr v-for="item in inventory" :key="item.id">
        <td>
          {{ item.product.name }}
        </td>
        <td v-bind:class="
        `${getColorForQauntityDisparity(item)}`
        ">
          {{ item.quantityOnHand }}
        </td>
        <td>
          {{ getPrice(item.product.price) }}
        </td>
        <td>
          <span v-if="item.product.isTaxable"> Yes </span>
          <span v-else> No </span>
        </td>
        <td>
          <i class="fa-regular fa-circle-xmark product-archive" 
          @click="archiveProduct(item.product.id)"></i>
        </td>
      </tr>
    </table>
    <new-product-modal
      v-if="isNewProductVisible"
      @save:product="saveNewProduct"
      @close="closeModals"
    />
    <shipment-modal
      v-if="isShipmentVisible"
      :inventory="inventory"
      @save:shipment="saveNewShipment"
      @close="closeModals"
    />
  </div>
</template>

<script lang="ts">
import { Options, Vue } from "vue-class-component";
import { IProductInventory, IProduct } from "@/types/Product";
import SolarButton from "@/components/SolarButton.vue";
import NewProductModal from "@/components/modals/NewProductModal.vue";
import ShipmentModal from "@/components/modals/ShipmentModal.vue";
import { IShipment } from "@/types/Shipment";
import { InventoryService } from "@/services/inventory-service";
import {ProductService} from "@/services/product-service";

const inventoryService = new InventoryService();
const productService = new ProductService();

@Options({
  name: "Inventory",
  components: { SolarButton, NewProductModal, ShipmentModal },
})
export default class Inventory extends Vue {
  isNewProductVisible = false;
  isShipmentVisible = false;
  
  inventory: IProductInventory[] = [];

  getPrice(number: number) {
    if (isNaN(number)) {
      return "-";
    } else {
      return "$ " + number.toFixed(2);
    }
  }

  async archiveProduct(productId: number){
    await productService.archive(productId)
    await this.initialize();
  }

  async saveNewProduct(newProduct: IProduct){
    await productService.save(newProduct);
    this.isNewProductVisible = false;
    await this.initialize();
  }
  
  closeModals() {
    this.isShipmentVisible = false;
    this.isNewProductVisible = false;
  }

  showNewProductModal() {
    this.isNewProductVisible = true;
  }

  showShipmentModal() {
    this.isShipmentVisible = true;
  }

  getColorForQauntityDisparity(product: IProductInventory) {

    let significantDifference = 8

    if(product.quantityOnHand <= 0){
      return "red";
    }
    if(Math.abs(product.idealQuantity - product.quantityOnHand) > significantDifference) {
      return "yellow"
    }
    return "green"
  }

  async saveNewShipment(shipment: IShipment) {
    await inventoryService.updateInventoryQuantity(shipment);
    this.isShipmentVisible = false;
    await this.initialize();
  }

  async initialize() {
    this.inventory = await inventoryService.getInventory();
  }

  async created() {
    await this.initialize();
  }
}
</script>

<style scoped lang="scss">
  @import "@/scss/global.scss";

  .green {
    font-weight: bold;
    color: $solar-green;
  }

  .yellow {
    font-weight: bold;
    color: $solar-yellow;
  }

  .red {
    font-weight: bold;
    color: $solar-red;
  }

  .inventory-actions{
    display: flex;
    margin-bottom: 0.8rem;
  }

  .product-archive{
    cursor: pointer;
    font-weight: bold;
    font-size: 1.2rem;
    color: $solar-red;
  }
</style>
