<script setup>
import { defineProps, defineEmits } from 'vue'

// const props = defineProps(['datas'])
defineProps(['datas'])
const emits = defineEmits(['add-to-order'])
</script>
<template>
  <div class="list-group">
    <a
      href="#"
      class="list-group-item list-group-item-action"
      v-for="data in datas"
      :key="data.id"
      @click.prevent="emits('add-to-order', data.id)"
    >
      <div class="d-flex w-100 justify-content-between">
        <h5 class="mb-1">{{ data.name }}</h5>
        <small>{{ `$${data.price}` }}</small>
      </div>
      <p class="mb-1">{{ data.description }}</p>
    </a>
  </div>
</template>
