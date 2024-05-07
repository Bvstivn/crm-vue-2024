<template>
  <tr>
    <td class="whitespace-nowrap py-4 pl-4 pr-3 text-sm sm:pl-0">
      <p class="font-medium text-gray-900">{{ nombreCliente }}</p>
      <p class="text-gray-500">{{ cliente.email }}</p>
    </td>
    <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
      <p class="text-gray-900 font-bold">{{ cliente.empresa }}</p>
      <p
        class="text-gray-600"
        :class="
          estadoCliente
            ? 'bg-green-100 text-green-800'
            : 'bg-red-100 text-red-800'
        "
      >
        <button
          class="inline-flex rounded-full px-2 text-xs font-semibold leading-5"
          @click="$emit('actualizar-estado', cliente.id)"
        >
          {{ cliente.puesto ? "Activo" : "Inactivo" }}
        </button>
      </p>
    </td>
    <td class="whitespace-nowrap px-3 py-4 text-sm">
      {{ estadoCliente }}
    </td>
    <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
      <RouterLink class="text-indigo-600 hover:text-indigo-900 mr-5" :to="{name: '/editar-cliente', params: {id: cliente.id} }"
        >Editar</RouterLink
      >
      <button class="text-red-600 hover:text-red-900" @click="$emit('eliminar-cliente', cliente.id)" >Eliminar</button>
    </td>
  </tr>
</template>

<script setup>
//Vue
import { computed } from "vue";
//Vue Router
import { RouterLink } from "vue-router";

//Definiciones
const props = defineProps({
  cliente: {
    type: Object,
    required: true,
  },
});

defineEmits(['actualizar-estado', 'eliminar-cliente']);

//Computed
const nombreCliente = computed(
  () => props.cliente.nombre + " " + props.cliente.apellido
);

const estadoCliente = computed(() => props.cliente.estado);
</script>
