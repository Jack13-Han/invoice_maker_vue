<template>
  <tr>
    <td>
      <i @click="$emit('toDel')" class="fa-solid fa-trash-alt text-danger"></i>
    </td>
    <td>{{ record.product.name }}</td>
    <td class="text-end">{{ record.product.price }}</td>
    <td class="text-end quantity-width" @dblclick="quantityEdit=true">
      <input  @keyup.enter="exitQuantityEdit" @change="updateQuantity" v-model="record.quantity" v-if="quantityEdit" type="number" min="1" class="form-control w-100">
      <span v-else>{{ record.quantity }}</span>
    </td>
    <td class="text-end">{{ record.cost }}</td>
  </tr>
</template>

<script>
export default {
  data() {
    return {
      quantityEdit: false,
    }
  },
  methods: {
    updateQuantity() {
      this.record.cost= this.record.product.price * this.record.quantity;

    },

    exitQuantityEdit(){
      this.quantityEdit= false;
    }
  },

  props: {
    record: {
      type: Object,
    },
  },
}
</script>

<style>
  .quantity-width{
    width: 150px;
  }
</style>