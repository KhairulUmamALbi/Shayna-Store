<template>
  <div class="product">
    <HeaderShayna />

    <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section text-left">
      <div class="container">
        <div class="row">
          <div class="col-lg-12">
            <div class="breadcrumb-text product-more">
              <router-link to="/"><i class="fa fa-home"></i> Home</router-link>
              <span>Detail</span>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- Breadcrumb Section Begin -->

    <!-- Product Shop Section Begin -->
    <section class="product-shop spad page-details">
      <div class="container">
        <div class="row">
          <div class="col-lg-12">
            <div class="row">
              <div class="col-lg-6">
                <div class="product-pic-zoom">
                  <img class="product-big-img" :src="gambar_default" alt="" v-lazy-load />
                </div>
                <div class="product-thumbs" v-if="productDetails.galleries.length> 0">
                  <carousel :autoplay="false" :loop="false" :dots="false" :nav="false" class="product-thumbs-track ps-slider ">
                    <div v-for="ss in productDetails.galleries" :key="ss.id" 
                    class="pt" v-lazy-load @click="changeImage(ss.photo)" 
                    :class="ss.photo == gambar_default ? 'active' : '' ">
                      <img :src="ss.photo" alt="" />
                    </div>

                  </carousel>
                </div>
              </div>
              <div class="col-lg-6">
                <div class="product-details text-left">
                  <div class="pd-title">
                    <span>{{ productDetails.type }}</span>
                    <h3>{{ productDetails.name }}</h3>
                  </div>
                  <div class="pd-desc">
                    <p v-html="productDetails.description">
                    </p>
                    <h4>${{ productDetails.price }}</h4>
                  </div>
                  <div class="quantity">
                    <router-link to="/cart">
                      <a @click="saveKeranjang(productDetails.id, productDetails.name,productDetails.price, productDetails.galleries[0].photo  )" class="primary-btn pd-cart" href="#">Add To Cart</a>
                    </router-link>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
    <RelatedShayna />
    <!-- Product Shop Section End -->
    <FooterShayna />
  </div>
</template>

<script>
// @ is an alias to /src
import HeaderShayna from '@/components/HeaderShayna.vue';

import FooterShayna from '@/components/FooterShayna.vue';
import RelatedShayna from '@/components/ProductComponents/RelatedShayna.vue';
import carousel from 'vue-owl-carousel';
import Vue from 'vue';
import axios from 'axios';

export default {
  name: 'ProductView',
  components: {
    HeaderShayna,
    FooterShayna,
    RelatedShayna,
    carousel
  },
  data(){
    return {
      gambar_default: '',
      productDetails: [],
      keranjangUser: []
    }
  },
  methods: {
    changeImage(urlImage) {
      this.gambar_default = urlImage;
      
    },
    setDataPicture(data){
      // replace object productDetails dengan data dari API
      this.productDetails = data;
      // replace value productDetails dengan data dari API{galleries}
      this.gambar_default = data.galleries[0].photo;
  },
    saveKeranjang(idProduct, nameProduct, priceProduct, photoProduct){

      var productStored = {
        "id": idProduct,
        "name": nameProduct,
        "price": priceProduct,
        "photo": photoProduct
      }


      this.keranjangUser.push(productStored);
      const parsed = JSON.stringify(this.keranjangUser);
      localStorage.setItem('keranjangUser', parsed);
  }
},
      mounted() {
        if(localStorage.getItem('keranjangUser')){
          try {
            this.keranjangUser = JSON.parse(localStorage.getItem('keranjangUser'));
          } catch (error) {
            localStorage.removeItem('keranjangUser');
          }
        }
        axios
            .get('https://shayna-backend.albikhairul.site/api/product', {
                params: {
                    id: this.$route.params.id
                }
            })
            .then(response => {
                this.setDataPicture(response.data.data);
            })
            .catch(error => {console.log(error);
            });
    }
}
// Lazy load directive
Vue.directive('lazy-load', {
  inserted: function (el) {
    const loadImage = () => {
      el.src = el.dataset.src;
      el.classList.add('loaded'); // Optional: Add a class for styling loaded images
      observer.disconnect();
    };

    const observer = new IntersectionObserver((entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          loadImage();
        }
      });
    });

    observer.observe(el);
  },
});
</script>

<style scoped>
.product-thumbs .pt{
  margin-right: 14px;
}
</style>