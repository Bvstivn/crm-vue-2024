<template>
    <div>
      <RouterLink :to="{ name: to }" class="block md:inline-block bg-blue-600 py-2 px-3 text-white uppercase font-bold text-sm shadow hover:bg-blue-500" >
        <slot></slot>
    </RouterLink>
    </div>
</template>

<script setup>
//Vue-router
import { RouterLink } from "vue-router";

//Deinificones
defineProps({
    to:{
        type: String,
        required: true,
    }
});
</script>