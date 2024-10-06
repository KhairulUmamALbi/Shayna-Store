<template>
        <!-- Women Banner Section Begin -->
        <section class="women-banner spad">
        <div class="container-fluid">
            <div class="row">
                <div class="col-lg-12 mt-5" v-if="product.length > 0 ">
                    <carousel class="product-slider" :items="3" :nav="false" :dots="false" :autoplay="true">
                        
                        <div class="product-item" v-for="itemProduct in product" :key="itemProduct.id" >
                            <div class="pi-pic">
                                <img :src="itemProduct.galleries[0].photo" alt="" />
                                <ul>
                                    <li @click="saveKeranjang(itemProduct.id, itemProduct.name,itemProduct.price, itemProduct.galleries[0].photo  )"
                                        class="w-icon active">
                                        <a href="#"><i class="icon_bag_alt"></i></a>
                                    </li>
                                    <li class="quick-view"><router-link :to="'/product/'+itemProduct.id">+ Quick View</router-link></li>
                                </ul>
                            </div>
                            <div class="pi-text">
                                <div class="catagory-name">{{ itemProduct.type }}</div>
                                <router-link to="/product">
                                <a href="#">
                                    <h5>{{ itemProduct.name }}</h5>
                                </a>
                            </router-link>
                                <div class="product-price">
                                    ${{ itemProduct.price }}
                                    <span>$35.00</span>
                                </div>
                            </div>
                        </div>


                    </carousel>
                </div>
                <div class="col-lg-12" v-else>
                    <p> Produk belum tersedia </p>
                </div>
            </div>
        </div>
    </section>
    
    <!-- Women Banner Section End -->
</template>

<script>
import carousel from 'vue-owl-carousel';
import axios from 'axios';

export default {
    name: 'WomenShayna',
    components: {
        carousel
    },
    data() {
        return {
            product: [],
            keranjangUser: []
        }
    },
    methods: {
        saveKeranjang(idProduct, nameProduct, priceProduct, photoProduct) {
            var productStored = {
                "id": idProduct,
                "name": nameProduct,
                "price": priceProduct,
                "photo": photoProduct
            }

            this.keranjangUser.push(productStored);
            const parsed = JSON.stringify(this.keranjangUser);
            localStorage.setItem('keranjangUser', parsed);

            window.location.reload();
        }
    },
    mounted() {

        axios
            .get('http://shayna-backend.albikhairul.site/api/product')
            .then(response => {this.product = response.data.data.data;})
            .catch(error => {console.log(error);
            });
        if(localStorage.getItem('keranjangUser')) {
            try {
                this.keranjangUser = JSON.parse(localStorage.getItem('keranjangUser'));
            } catch(e) {
                localStorage.removeItem('keranjangUser');
            }
        }
    }
}
</script>

<style scoped>
.product-item {
    margin-right: 25px;
}
</style>