<script>
import { mapActions, mapState, mapWritableState } from 'pinia';
import { useCounterStore } from '../stores/counter';
import Swal from 'sweetalert2'


export default{
  data(){
    return {
      productId: this.product.id,
      amount:0,
      price:this.product.price,
    }
  },
  components: {
  },
  computed: {
    ...mapState(useCounterStore, []),
    ...mapWritableState(useCounterStore, [])
  },
  methods: {
    ...mapActions(useCounterStore, ['postCart']),
    async handlePostCart(){
      try {
        if (!this.amount) {
          Swal.fire({
            icon: 'error',
            text: 'Please input the amount!',
          })
        }
        else if (this.amount > 0) {
          let value = {
            productId: this.productId,
            amount: this.amount,
            price: this.price,
          }
          await this.postCart(value)
          this.amount = 0
        } 
        else {
          Swal.fire({
            icon: 'error',
            text: 'Please input the proper amount!',
          })
        }
      } catch (error) {
        
      }
      
    }
  },
  props: ['product'],
  created(){
    
  },
  mounted(){
  }
}
</script>

<template>
    <div class="col-xl-4 col-md-6 col-sm-12">
        <div class="card">
            <div class="card-content d-flex flex-column align-items-stretch">
                <div class="card-body blog-title">
                    <h4 class="card-title">{{product.name}}</h4>
                </div>
                <img class="img-fluid w-100 blog-image" :src="product.image"
                    alt="Card image cap">
            </div>
            <div class="card-footer d-flex justify-content-between">
                <div class="blog-category py-1 px-2 rounded-xl row align-items-center">
                    Rp. {{product.price}}
                </div>
                <div @click.prevent="handlePostCart" class="fig stats-icon red mb-2">
                    <i class="bi-cart-plus"></i>
                </div>
            </div>
            <div class="card-footer d-flex justify-content-between">
              <form >
                <input v-model="amount" type="number" id="amount" name="amount" width="3" placeholder="Amount">
              </form>
            </div>
        </div>
    </div>
</template>

<style scoped>
.fig {
  /* background-color: DodgerBlue; Blue background */
  border: none; /* Remove borders */
  color: white; /* White text */
  padding: 12px 16px; /* Some padding */
  font-size: 16px; /* Set a font size */
  cursor: pointer; /* Mouse pointer on hover */
}

/* Darker background on mouse-over */
.fig:hover {
  background-color: RoyalBlue;
}

.blog-category {
	background: rgba(40, 167, 69, 0.1);
	color: #28a745;
    border-radius: 10px;
}

.blog-title {
  width: 200px;
	height: 150px;
}

.blog-image {
	width: 100px;
	height: 300px;
	object-fit: contain;
}
</style>