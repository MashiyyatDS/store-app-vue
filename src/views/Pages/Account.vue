<template>
    <OffCanvas/>
    <div class="row">
      <Navbar/>
      <div class="col-lg-3">
        <Sidebar/>
      </div>
      <!-- SIDEBAR -->
      <div class="col-lg-9 col-md-12 mt-2">
        <ProfileTab/>

        <div class="m-1">
        <NavTabs 
          v-bind:tab=" 'orders' "/>
        </div>
        <div class="m-1">
          <OrderList />
        </div>
        <div class="m-1">
          <div class="d-flex justify-content-center">
            <Pagination
              v-bind:linkData="orderLinks"
              v-on:emitLink="fetchOrders"/>
          </div>
        </div>
      </div>
      <!-- CONTENT -->
    </div>
</template>

<script>
import OffCanvas from '@/components/Partials/OffCanvas'
import Navbar from '@/components/Partials/Navbar'
import Sidebar from '@/components/Partials/Sidebar'
import NavTabs from '@/components/Partials/NavTabs'
// import OrderItem from '@/components/Partials/OrderItem'
import Pagination from '@/components/Partials/Pagination'
import ProfileTab from '@/components/Partials/ProfileTab'
import OrderList from '@/components/Partials/OrderList'
import { mapGetters, mapActions } from 'vuex'

export default {
  name: 'Account',
  components: {
    OffCanvas, 
    Navbar, 
    Sidebar, 
    NavTabs, 
    // OrderItem, 
    Pagination,
    ProfileTab,
    OrderList
  },
  data() {
    return {
      url: 'api/orders/status/on-process'
    }
  },
  computed: {
    ...mapGetters(['user', 'orders', 'orderLinks'])
  },
  methods: {
    ...mapActions(['fetchOrders'])
  },
  created() {
    this.fetchOrders(this.url)
  }
}
</script>