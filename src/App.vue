<template>
  <div class="container">
      <div class="row justify-content-center align-items-center">
        <div class="col-10">
          <h3 class="text-primary fw-bold text-center my-3">
            Invoice Maker
            <a href="" target="_blank">
              <i class="fa-solid fa-file-circle-plus"></i>
            </a>
          </h3>

          <form action="" class=""  @submit.prevent="saveRecord">
            <div class="row g-1">
              <div class="col-6">
                <select v-model="selectedProduct"  class="form-select " required>
                  <option value="">Select Product</option>
                  <option v-for="product in products" :key="product.id" :value="product.id" required>
                    {{ product.name }} (${{ product.price }})
                  </option>
                </select>
              </div>

              <div class="col">
                <input type="number" min="1" v-model="inputQuantity" class="form-control" placeholder="Quantity" required>
              </div>

              <div class="col">
                <button class="btn btn-primary w-100">
                  <i class="fa-solid fa-plus"></i>
                </button>
              </div>
            </div>
          </form>

          <table class="table table-hover table-bordered">
            <thead>
              <tr>
                <th>#</th>
                <th>Product Name</th>
                <th>Unit Price</th>
                <th>Quantity</th>
                <th>Cost</th>
              </tr>
            </thead>

            <tbody>
              <tr v-show="records.length === 0">
                <td colspan="5" class="text-center">Not Record Yet</td>
              </tr>

              <Row @to-del="del(index)"  v-for="(record,index) in records" :key="index" :record="record"></Row>

            </tbody>

            <tfoot v-show="records.length > 0">
            <tr>
              <td colspan="4" class="text-center fw-bolder">
                Total Cost
              </td>

              <td>
                {{ calcTotalCost }}
              </td>
            </tr>
            </tfoot>
          </table>
          
          <div class="mt-3 text-center">
            <button class="btn btn-outline-primary me-2" @click="print">Print</button>
            <button class="btn btn-primary" @click="save">Save</button>
          </div>

        </div>
      </div>
  </div>
</template>

<script>
import Row from "@/components/Row";
export default {
  components: {Row},
  data() {
    return {
      selectedProduct : '',
      inputQuantity : '',
      products: [
        {
          id : 1,
          name : 'Apple',
          price : 50,
        },
        {
          id : 2,
          name : "Orange",
          price : 55
        },
        {
          id : 3,
          name : "Mango",
          price : 150
        }
      ],
      records : []
    }
  },

  computed: {
    calcTotalCost() {
      return this.records.reduce( (p,c) => p + c.cost ,0)
    }
  },
  
  methods: {
    saveRecord() {
      let currentProduct = this.products.find(el=> el.id === this.selectedProduct);
      let calcCost = currentProduct.price * this.inputQuantity;
      let record = {
        product : currentProduct,
        quantity : this.inputQuantity,
        cost : calcCost,
      };
      this.records.push(record);
      this.selectedProduct = this.inputQuantity ="" ;

      // console.log(record);
    },
    del(index){
      this.records.splice(index,1)
      console.log(index)
    },
    print(){
      window.print();
    },
    save(){
      this.records=[];
    }
  },


}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@200;400;700&family=Padauk:wght@400;700&display=swap');

$font-family-sans-serif: 'Montserrat','Padauk',sans-serif;

@import "~bootstrap/scss/bootstrap.scss";
@import "~@fortawesome/fontawesome-free/css/all.min.css";
</style>