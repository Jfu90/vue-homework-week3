<script setup>
import { defineProps, defineEmits, ref, computed } from 'vue'

const props = defineProps(['select-list'])
const emits = defineEmits(['del-order-item', 'create-order'])

const orderMemo = ref([])
const newCreateOrder = () => {
  emits('create-order', orderMemo.value)
  orderMemo.value = ''
}
const computedTotal = computed(() => {
  return props.selectList.reduce((money, item) => money + item.subtotal, 0)
})
</script>

<template>
  <table class="table">
    <thead>
      <tr>
        <th scope="col" width="50">操作</th>
        <th scope="col">品項</th>
        <th scope="col">描述</th>
        <th scope="col" width="90">數量</th>
        <th scope="col">單價</th>
        <th scope="col">小計</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="product in selectList" :key="product.id">
        <td>
          <button type="button" class="btn btn-sm" @click="emits('del-order-item', product.id)">
            x
          </button>
        </td>
        <td>{{ product.name }}</td>
        <td>
          <small>{{ product.description }}</small>
        </td>
        <td>
          <select
            class="form-select"
            v-model="product.count"
            @change="product.subtotal = product.price * product.count"
          >
            <option v-for="i of 10" :key="i" :value="i">{{ i }}</option>
          </select>
        </td>
        <td>{{ product.price }}</td>
        <td>{{ product.subtotal }}</td>
      </tr>
    </tbody>
  </table>
  <div class="text-end mb-3">
    <h5>
      總計: <span>${{ computedTotal }}</span>
    </h5>
  </div>
  <textarea class="form-control mb-3" rows="3" placeholder="備註" v-model="orderMemo"></textarea>
  <div class="text-end">
    <button class="btn btn-primary" @click.prevent="newCreateOrder">送出</button>
  </div>
</template>
