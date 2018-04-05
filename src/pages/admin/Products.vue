<template>
    <div class="col-lg-9 col-md-9 col-sm-12 col-xs-12">
          <table class="table table-striped table-bordered table-responsive">
            <thead>
            <tr>
              <th><input type="checkbox" @click="selectAll" v-model="allSelected"></th>
              <th>Name</th>
              <th>Price</th>
              <th>Manufacturer</th>
              <th colspan="2" style="text-align:center">Action</th>
            </tr>
            </thead>
            <tbody>
            <tr v-for="product in products" :key="product._id" :class="(checkArray(product._id) ? 'checked': 'unchecked')">
              <td><input type="checkbox" :value="product._id"  v-model="productIds"></td>
              <td>{{product.name}}</td>
              <td>${{product.price}}</td>
              <td>{{product.manufacturer.name}}</td>
              <td><router-link :to="'/admin/edit/'+product._id"><i class="fa fa-pencil-square-o"></i></router-link></td>
              <td><a @click="deleteProduct(product._id)"><i class="fa fa-trash"></i></a></td>
            </tr>
            </tbody>
          </table>
        </div>
</template>

<script>
  export default {
    data() {
        return{ 
          selected: [],
          allSelected: false,
          productIds: []
        }
    },
    computed: {
      products () {
        return this.$store.getters.allProducts
      }
    },
    created () {
      if (this.products.length === 0) {
        this.$store.dispatch('allProducts')
      }
    },
    methods: {
      deleteProduct (id) {
        this.$store.dispatch('removeProduct', id)
      },
      selectAll: function() {
            this.productIds = [];
            if (!this.allSelected) {
                for (var product in this.products) {
                    this.productIds.push(this.products[product]._id);
                }
            }
        },
      checkArray: function(id) {
          if(this.productIds.indexOf(id) !== -1)
          {
            return true;
          }
          else{
            return false;
          }
      }
    }
  }
</script>

<style>
  .checked{
    background:rgb(189, 151, 151) !important;
    color:red;
  }
</style>
