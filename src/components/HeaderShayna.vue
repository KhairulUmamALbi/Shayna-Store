<template>
<div>
        <!-- Header Section Begin -->
        <header class="header-section">
        <div class="header-top">
            <div class="container">
                <div class="ht-left">
                    <div class="mail-service">
                        <i class=" fa fa-envelope"></i> hello.e-commerce@gmail.com
                    </div>
                    <div class="phone-service">
                        <i class=" fa fa-phone"></i> +628 0000000
                    </div>
                </div>
            </div>
        </div>
        <div class="container">
            <div class="inner-header">
                <div class="row">
                    <div class="col-lg-2 col-md-2">
                        <div class="logo">
                            <a href="./index.html">
                                <img src="img/logo_website_shayna.png" alt="" />
                                 <!-- <h3>Shayna</h3> -->
                            </a>
                        </div>
                    </div>
                    <div class="col-lg-7 col-md-7"></div>
                    <div class="col-lg-3 text-right col-md-3">
                        <ul class="nav-right">
                            <li class="cart-icon">
                                Keranjang Belanja &nbsp;
                                <a href="#">
                                    <i class="icon_bag_alt"></i>
                                    <span>{{ keranjangUser.length }}</span>
                                </a>
                                <div class="cart-hover">
                                    <div class="select-items">
                                        <table>
                                            <tbody v-if="keranjangUser.length > 0">
                                                <tr v-for="keranjang in keranjangUser" :key="keranjang.id">
                                                    <td class="si-pic">
                                                        <img class="photo-item" :src="keranjang.photo" alt="" />
                                                    </td>
                                                    <td class="si-text">
                                                        <div class="product-selected">
                                                            <p>${{ keranjang.price }}</p>
                                                            <h6>{{ keranjang.name }}</h6>
                                                        </div>
                                                    </td>
                                                    <td @click="removeItem(keranjang.id)" class="si-close">
                                                        <i class="ti-close"></i>
                                                    </td>
                                                </tr>
                                            </tbody >
                                            <tbody v-else>
                                                <tr>
                                                    <td>Keranjang Kosong</td>
                                                </tr>
                                            </tbody>
                                        </table>
                                    </div>
                                    <div class="select-total">
                                        <span>total:</span>
                                        <h5>${{ totalHarga }}.00</h5>
                                    </div>
                                    <!-- <div class="select-button">
                                        <router-link to="/cart">
                                        <a href="#" class="primary-btn view-card">VIEW CARD</a>
                                    </router-link>
                                        <a href="#" class="primary-btn checkout-btn">CHECK OUT</a>
                                    </div> -->
                                    <div class="select-button">
                                        <router-link to="/cart" class="primary-btn view-card" tag="a">VIEW CARD</router-link>
                                        <a href="#" class="primary-btn checkout-btn">CHECK OUT</a>
                                    </div>
                                </div>
                            </li>
                        </ul>
                    </div>
                </div>
            </div>
        </div>
    </header>
    <!-- Header End -->

</div>
</template>

<script>
// import router from '@/router';

export default {
    name: 'HeaderShayna',
    data(){
    return {

        keranjangUser: []
    };
  },
  methods: {
    removeItem(idx){
    //   this.keranjangUser.splice(index, 1); // menghapus data keranjang yang dipilih
    //   const parsed = JSON.stringify(this.keranjangUser);
    //   localStorage.setItem('keranjangUser', parsed);

    // cari tahulah id dari item yang akan dihapus
    let KeranjangUserStored = JSON.parse(localStorage.getItem('keranjangUser'));
    let itemKeranjangUserStored = KeranjangUserStored.map(itemKeranjangUserStored => itemKeranjangUserStored.id);
    
    // mencocokan id yang akan dihapus
    let index = itemKeranjangUserStored.findIndex(id => id == idx);
    this.keranjangUser.splice(index, 1);
    
    const parsed = JSON.stringify(this.keranjangUser);
    localStorage.setItem('keranjangUser', parsed);
    window.location.reload();

    }
  },
  mounted(){
    if(localStorage.getItem('keranjangUser')){ // jika ada data di localstorage maka diambil
          try {
            this.keranjangUser = JSON.parse(localStorage.getItem('keranjangUser'));
          } catch (error) {
            localStorage.removeItem('keranjangUser');
          }
        }
  },
  computed: {
    totalHarga(){
        return this.keranjangUser.reduce(function(items, data){ // menghitung total harga
            return items + data.price;
        }, 0);
  }
},
};
</script>

<style>
.photo-item {
    width: 80px;
    height: 80px;
}
</style>