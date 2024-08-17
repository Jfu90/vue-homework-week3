<script setup>
import { ref } from 'vue'
import axios from 'axios'

import ProductList from './components/ProductList.vue'
import SelectList from './components/SelectList.vue'
import OrderList from './components/OrderList.vue'

const selectList = ref([])
const orderList = ref([])

const datas = ref([])
const products = async () => {
  try {
    const response = await axios.get('./data.json')
    datas.value = response.data
  } catch (error) {
    console.log(error)
  }
}
products()

const addToOrder = (id) => {
  const idx = datas.value.findIndex((item) => item.id === id)
  const haveItemIdx = selectList.value.findIndex((item) => item.id === id)
  if (haveItemIdx >= 0) {
    const item = selectList.value[haveItemIdx]
    if (item.count < 10) {
      item.count++
      item.subtotal = item.price * item.count
    } else false
  } else {
    selectList.value.push({ ...datas.value[idx], count: 1, subtotal: datas.value[idx].price })
  }
}

const delOrderItem = (id) => {
  const idx = selectList.value.findIndex((item) => item.id === id)
  selectList.value.splice(idx, 1)
}

const createOrder = (msg) => {
  orderList.value = {
    list: { ...selectList.value },
    memo: msg,
    total: selectList.value.reduce((a, b) => a + b.subtotal, 0)
  }
  selectList.value.length = 0
}
</script>

<template>
  <main>
    <div id="root">
      <div class="container mt-5">
        <div class="row">
          <div class="col-md-4">
            <ProductList :datas="datas" @add-to-order="addToOrder" />
          </div>
          <div class="col-md-8">
            <SelectList
              :select-list="selectList"
              @del-order-item="delOrderItem"
              @create-order="createOrder"
            />
          </div>
        </div>
        <hr />
        <div class="row justify-content-center">
          <div class="col-8">
            <OrderList :order-list="orderList" />
          </div>
        </div>
      </div>
    </div>
  </main>
</template>
