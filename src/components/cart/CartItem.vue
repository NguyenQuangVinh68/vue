<template>

    <div class="row mt-5 cartItem-md d-flex align-items-center">
        <div class="col-lg-11 col-md-11">
            <div class="row d-flex align-items-center">
                <div class="col-lg-4 col-md-2 ">
                    <img :src="require(`@/assets/image/products/${item.image}`)" alt="" class="w-50">
                </div>
                <div class="col-lg-2 col-md-3 ">{{ item.name }}</div>
                <!-- <div class="col-lg-2 col-md-2 ">${{item.price.toFixed(2)}}</div> -->
                <div class="col-lg-2 col-md-2 ">
                    <div>
                        <p class="m-0" v-if="item.sale != 0">
                            <s class="old-price fs-6 fw-bold text-secondary">${{ item.price.toFixed(2) }}</s>
                            <span class="fw-bolder fs-6 price">&ensp;${{ item_discount.toFixed(2) }}</span>
                        </p>
                        <p class="m-0" v-else>
                            <span class="fw-bolder fs-6 price">${{ item.price.toFixed(2) }}</span>
                        </p>
                    </div>
                </div>
                <div class="col-lg-2 col-md-3 ">
                    <button class="btn border border-info" @click="removeItemCart(item)">-</button>
                    {{ item.quantity }}
                    <button :disabled="isDisabled" class="btn border border-info" @click="addToCart(item)">+</button>
                </div>
                <div class="col-lg-2 col-md-2 ">${{ item_discount_cost.toFixed(2) }}</div>
            </div>
        </div>
        <div class="col-lg-1 col-md-1 remove_item ">
            <font-awesome-icon :icon="['fas', 'xmark']" class="fs-2 " @click="clearItemCart(item)" style="cursor: pointer;" />
        </div>

    </div>
    <hr>
</template>

<script>
import { mapMutations } from 'vuex';


export default {
    name: "CartItem",
    components: {},
    props: ['item'],

    data() {
        return {
            isDisabled: false,
            discount: null
        }
    },
    created() {

    },
    methods: {
        ...mapMutations(['clearItemCart']),

        addToCart(product) {

            // product n??y ???????c truy???n t??? components l?? d??? li???u l???y t??? gi??? h??ng ra
            // => m?? khi th??m v??o gi??? h??ng ta ???? s??? l?? stock r???i => n???u stock n??y  == 0 <=> stock trong data == 0 (stock trong data khi b??? hao h???t khi th??m v??o gi??? h??ng)

            if (this.$store.state.isLoggedIn) {
                if (product.stock == 0) {
                    this.isDisabled = true;
                    alert("Quantity buy bigger than stock")
                } else {
                    this.$store.commit("addToCart", product)
                    this.isDisabled = false;
                }
            } else {
                alert("B???n c???n ????ng nh???p khi mua h??ng")
                this.$router.push("/login")
            }
        },
        // addToCart(product) {
        //     this.currentQuantity = product.quantity;
        //     if (this.currentQuantity == product.stock) {
        //         this.isDisabled = true;
        //         alert("H???t H??ng")
        //     } else {
        //         if (this.$store.state.isLoggedIn) {
        //             this.$store.commit("addToCart", product)
        //         } else {
        //             alert("B???n c???n ????ng nh???p khi mua h??ng")
        //             this.$router.push("/login")
        //         }
        //         this.isDisabled = false;
        //     }
        // },
        removeItemCart(product) {
            this.isDisabled = false
            this.$store.commit('removeItemCart', product)
        }
    },
    computed: {
        // t???ng ti???n chi cho m???t m???t h??ng
        item_discount_cost() {
            if (this.item.sale != 0) {
                return (((100 - this.item.sale) / 100) * this.item.price) * this.item.quantity
            }
            return this.item.price * this.item.quantity
        },
        // t??nh ti???n khuy???n m??i 
        item_discount() {
            return ((100 - this.item.sale) / 100) * this.item.price
        },
        itemCart() {
            return this.$store.state.cart
        }
    },
}
</script>

<style>
@media only screen and (max-width: 766px) {
    .cartItem-md {
        position: relative;
    }

    .remove_item {
        position: absolute !important;
        text-align: right;
        right: 15%;
    }

}
</style>


