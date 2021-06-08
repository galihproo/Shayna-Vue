<template>
  <div class="product">
    <HeaderShayna />
    <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section">
        <div class="container">
            <div class="row">
                <div class="col-lg-12">
                    <div class="breadcrumb-text product-more text-left">
                        <router-link to="/">
                            <i class="fa fa-home"></i> Home
                        </router-link>
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
                                <img class="product-big-img" :src="default_image" alt="" />
                            </div>
                            <div class="product-thumbs" v-if="productDetails.galleries.length > 0">
                                <carousel class="product-thumbs-track ps-slider" :nav="false" :dots="false" :items="3">
                                    <div v-for="thumb in productDetails.galleries" :key="thumb.id" class="pt" @click="changeImage(thumb.photo)" :class="thumb.photo == default_image  ? 'active' : '' ">
                                        <img :src="thumb.photo" class="thumbnail" />
                                    </div>
                                </carousel>
                            </div>
                        </div>
                        <div class="col-lg-6">
                            <div class="product-details">
                                <div class="pd-title text-left">
                                    <span>{{ productDetails.type }}</span>
                                    <h3>{{ productDetails.name }}</h3>
                                </div>
                                <div class="pd-desc text-left">
                                    <p v-html="productDetails.description" class="description"></p>
                                    <h4>Rp {{ productDetails.price }}</h4>
                                </div>
                                <div class="quantity text-left">
                                    <router-link to="/cart">
                                        <a @click="saveKeranjang(productDetails.id, productDetails.name, productDetails.price, productDetails.galleries[0].photo)" href="#" class="primary-btn pd-cart">Add To Cart</a>
                                    </router-link>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
    <!-- Product Shop Section End -->
    <RelatedProducts />
    <Footer />
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
import HeaderShayna from '@/components/HeaderShayna.vue';
import carousel from 'vue-owl-carousel';
import RelatedProducts from '@/components/RelatedProducts.vue';
import Footer from '@/components/Footer.vue';

import axios from 'axios';

export default {
  name: 'product',
  components: {
    HeaderShayna,
    carousel,
    RelatedProducts,
    Footer
  },
  data() {
      return {
          default_image: "",
          productDetails: [],
          keranjangUser: []
      }
  },
    methods: {
        changeImage(urlImage) {
            this.default_image = urlImage;
        },
        setDataPicture(data) {
            // replace object productDetails dengan data dari API
            this.productDetails = data;
            // replace valu gambar default dengan data dari API (galleries)
            this.default_image = data.galleries[0].photo;
        },
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
        }
    },
    mounted() {
        if (localStorage.getItem('keranjangUser')) {
            try {
                this.keranjangUser = JSON.parse(localStorage.getItem('keranjangUser'));
            } catch(e) {
                localStorage.removeItem('keranjangUser');
            }
        }

        axios
        .get('http://127.0.0.1:8000/api/products', {
            params: {
                id: this.$route.params.id
            }
        })
        .then(res => (this.setDataPicture(res.data.data)))
        .catch(err => console.log(err));
    }
}
</script>

<style scoped>
    .product-thumbs .pt {
        margin-right: 10px;
    }
    .product-thumbs .thumbnail {
        height: 260px;
        object-fit: cover;
    }
</style>
