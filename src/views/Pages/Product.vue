<template>
    <OffCanvas/>
    <div class="row">
      <Navbar/>
      <div class="col-lg-3">
        <Sidebar/>
      </div>
      <!-- SIDEBAR -->
      <div class="col-lg-9 col-md-12 mt-2">
        <div class="row">
          <div class="col-md-6 p-1">
            <div class="product-image">
              <div id="carouselExampleControls" class="carousel slide" data-bs-ride="carousel">
                <div class="carousel-inner">
                  <div class="carousel-item"
                    v-for="(image, index) in product.images" 
                    :key="image.id"
                    v-bind:class="{ 'active' : index == 0 }">
                    <img v-bind:src="image.image" class="d-block w-100" alt="...">
                  </div>
                </div>
                <button class="carousel-control-prev" type="button" data-bs-target="#carouselExampleControls" data-bs-slide="prev">
                  <span class="carousel-control-prev-icon" aria-hidden="true"></span>
                  <span class="visually-hidden">Previous</span>
                </button>
                <button class="carousel-control-next" type="button" data-bs-target="#carouselExampleControls" data-bs-slide="next">
                  <span class="carousel-control-next-icon" aria-hidden="true"></span>
                  <span class="visually-hidden">Next</span>
                </button>
              </div>
            </div>
          </div>
          <div class="col-md-6 p-1">
            <ul class="list-group product-details mb-3">
              <li class="list-group-item">
                <span class="name">{{ product.name }}</span>
              </li>
              <li class="list-group-item">
                Price: <span class="price"> ₱  {{ product.price }}</span>
              </li>
              <li class="list-group-item">
                Shipping fee: <span class="shipping-fee">₱  {{ product.shipping_fee }}</span>
              </li>
              <li class="list-group-item">
                Available stock: <span class="stock">{{ product.stock }} pcs</span>
              </li>
              <li class="list-group-item" v-for="variant in product.variants" :key="variant.id">
                {{ variant.title }}:
                <button class="btn btn-sm btn-outline-dark ms-1 mb-1" disabled v-for="item in variant.items" :key="item.id">
                  {{ item.name }}
                </button>
              </li>
              <li class="list-group-item">
                <div class="row">
                  <div class="col-6 p-1">
                    <div class="d-grid">
                      <button 
                        v-if="isAuthenticated"
                        class="btn btn-sm btn-outline-success" 
                        data-bs-toggle="modal" 
                        data-bs-target="#addToCartModal"
                        :class="{ 'disabled btn-outline-dark' : product.status != 'active' }">
                        Add to cart
                      </button>
                      <router-link to="/login" v-else 
                        class="btn btn-outline-dark btn-sm">
                        Login to your account
                      </router-link>
                    </div>
                  </div>
                  <div class="col-6 p-1">
                    <div class="d-grid">
                      <button class="btn btn-sm btn-outline-primary" @click="saveProduct(product.id)">
                        Save product
                      </button>
                    </div>
                  </div>
                </div>
              </li>
            </ul>
          </div>
        </div>
        <div class="row">
          <div class="col-md-12 p-1 mb-3">
            <ul class="list-group">
              <li class="list-group-item">
                <small class="text-muted">Tags: </small><br>
                <button class="btn btn-sm btn-outline-dark mb-1 ms-1 disabled" 
                  v-for="tag in product.tags" 
                  :key="tag.id">
                  {{ tag.tag }}
                </button>
              </li>
              <li class="list-group-item">
                <small class="text-muted">Categories: </small><br>
                <button class="btn btn-sm btn-outline-dark mb-1 ms-1 disabled"
                  v-for="category in product.categories"
                  :key="category.id">
                  {{ category.category }}
                </button>
              </li>
              <li class="list-group-item">
                <small class="text-muted">Description: </small> <br>
                {{ product.description }}
              </li>
            </ul>
          </div>
        </div>
        <CartAddModal 
          v-bind:product="product"/>
      </div>
      <!-- CONTENT -->
    </div>
</template>

<script>
import OffCanvas from '@/components/Partials/OffCanvas'
import Navbar from '@/components/Partials/Navbar'
import Sidebar from '@/components/Partials/Sidebar'
import CartAddModal from '@/components/Partials/CartAddModal'
import { mapGetters, mapActions } from 'vuex'

export default {
  name: 'Product',
  components: {
    OffCanvas, Navbar, Sidebar, CartAddModal
  },
  computed: {
    ...mapGetters(['product', 'isAuthenticated'])
  },
  methods: {
    ...mapActions(['findProduct', 'saveProduct']),
  },
  created() {
    this.findProduct(this.$route.params.slug)
  }
}
</script>


<style scoped>
  .product-image img{
    width: 100%;
    border-radius: 3px;
  }
  .product-details .name {
    font-size: 23px;
    font-family: 'Sen';
  }
  .product-details .price, .shipping-fee, .stock {
    font-size: 15px;
    font-family: 'Lexend';
    color: green;
  }
</style>