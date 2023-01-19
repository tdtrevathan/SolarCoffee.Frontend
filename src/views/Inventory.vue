<template>
    <div class="inventory-container">
        <h1 id="inventoryTitle">
            Inventory Dashboard
        </h1>
        <hr/>

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
                <td>
                    {{ item.quantityOnHand }}
                </td>
                <td>
                    {{ item.product.price }}
                </td>
                <td>
                    {{ item.product.isTaxable}}
                </td>
                <div>
                    x
                </div>
            </tr>
        </table>
    </div>
</template>

<script lang="ts">
    import {Options, Vue} from 'vue-class-component';
    import {IProductInventory} from '@/types/Product';
    @Options({
        name: "Inventory",
        components: {}
    })

    export default class Inventory extends Vue{
        inventory: IProductInventory[] = [
            {
                id:1,
                product: {
                    id: 1,
                    name: 'Wurther\'s origionals',
                    description: 'Canidies for all occasions',
                    price: 5,
                    createdOn: new Date(),
                    updatedOn: new Date(),
                    isTaxable: true,
                    isArchived: false
                },
                idealQuantity: 5,
                quantityOnHand: 2
            },
            {
                id:2,
                product: {
                    id: 2,
                    name: 'Mug',
                    description: 'For coffee',
                    price: 2,
                    createdOn: new Date(),
                    updatedOn: new Date(),
                    isTaxable: true,
                    isArchived: false
                },
                idealQuantity: 500,
                quantityOnHand: 1000
            },
        ]
    }
</script>

<style scoped>

</style>