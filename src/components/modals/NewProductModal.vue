<template>
    <solar-modal>
        <template v-slot:header>
            Add New Product
        </template>
        <template v-slot:body>
            <ul class="newProduct">
                <li>
                    <label for="isTaxable">Is this product taxable?</label>
                    <input type="checkbox" id="isTaxable" v-model="newProduct.isTaxable">
                </li>
                <li>
                    <label for="productName">Name</label>
                    <input type="text" id="productName" v-mdoel="newProduct.name">
                </li>
                <li>
                    <label for="productDesc">Description</label>
                    <input type="text" id="productDesc" v-model="newProduct.description">
                </li>
                <li>
                    <label for="productPrice">Price (USD)</label>
                    <input type="number" id="productPrice" v-model="newProduct.price">
                </li>
            </ul>
        </template>
        <template v-slot:footer>
            <solar-button type="button" @click.native="save" aria-label="save new item">
                Save Product
            </solar-button>
            <solar-button type="button" @click.native="close" aria-label="close modal">
                Close
            </solar-button>
        </template>
    </solar-modal>
</template>

<script lang="ts">
    import { Options, Vue } from "vue-class-component";
    import SolarButton from "@/components/SolarButton.vue";
    import SolarModal from "@/components/modals/SolarModal.vue";
    import { Prop } from "vue-property-decorator";
    import { IProduct, IProductInventory } from "@/types/Product";

    @Options({
        name: 'NewProductModal',
        components: { SolarButton, SolarModal }
    })

    export default class NewProductModal extends Vue{
        @Prop({required: true, type: Array as () => IProductInventory[] })
        inventory!: IProductInventory[];

        newProduct: IProduct = {
            createdOn: new Date(),
            updatedOn: new Date(),
            id: 0,
            description: "",
            isTaxable: false,
            name: "",
            price: 0,
            isArchived: false
        };

        qtyReceived: number = 0;

        close(){
            this.$emit('close');
        }

        save(){
            this.$emit('save:product', this.newProduct);
        }
    }
</script>

<style scoped lang="scss">
    .newProduct{
        list-style: none;
        padding: 0;
        margin: 0;

        input{
            width: 100%;
            height: 1.8rem;
            margin-bottom: 1.2rem;
            font-size: 1.1rem;
            line-height: 1.3rem;
            padding: 0.2rem;
            color: #555;
        }
        label{
            font-weight: bold;
            display: block;
            margin-bottom: 0.3rem;
        }
    }
</style>