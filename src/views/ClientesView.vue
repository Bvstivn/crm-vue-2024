<template>
  <div>
    <div>
      <RouterLinkVue to="agregar-cliente"> Agregar Cliente </RouterLinkVue>
    </div>
    <HeadingVue>{{ titulo }}</HeadingVue>
    <div
      v-if="existenClientes"
      class="flow-root mx-auto mt-10 p-5 bg-white shadow"
    >
      <div class="-my-2 -mx-4 overflow-x-auto sm:-mx-6 lg:-mx-8">
        <div class="min-w-full py-2 align-middle sm:px-6 lg:px-8">
          <table class="min-w-full divide-y divide-gray-300">
            <thead>
              <tr>
                <th
                  scope="col"
                  class="p-2 text-left text-sm font-extrabold text-gray-600"
                >
                  Nombre
                </th>
                <th
                  scope="col"
                  class="p-2 text-left text-sm font-extrabold text-gray-600"
                >
                  Empresa
                </th>
                <th
                  scope="col"
                  class="p-2 text-left text-sm font-extrabold text-gray-600"
                >
                  Estado
                </th>
                <th
                  scope="col"
                  class="p-2 text-left text-sm font-extrabold text-gray-600"
                >
                  Acciones
                </th>
              </tr>
            </thead>
            <tbody class="divide-y divide-gray-200 bg-white">
              <Cliente 
                v-for="cliente in clientes"
                :key="cliente.id"
                :cliente="cliente"
                @actualizar-estado="actualizarEstado"
                @eliminar-cliente="eliminarCliente"
              />
            </tbody>
          </table>
        </div>
      </div>
    </div>
    <p v-else class="text-center mt-10">No hay clientes</p>
  </div>
</template>

<script setup>
//Vue
import { onMounted, ref, computed } from "vue";
//Components
import RouterLinkVue from "@/components/UI/RouterLink.vue";
import HeadingVue from "@/components/UI/Heading.vue";
import Cliente from "@/components/Cliente.vue";
//Services
import ClienteService from "@/services/ClienteService";

//Data
const clientes = ref([]);

//onMounted
onMounted(() => {
  ClienteService.obtenerClientes()
    .then(({ data }) => (clientes.value = data))
    .catch((error) => {
      console.log(error);
    });
});

//Definiciones
defineProps({
  titulo: {
    type: String,
    required: true,
  },
});

//Computed
const existenClientes = computed(() => clientes.value.length > 0);

//Methods
const actualizarEstado = ({id, estado}) => {
  ClienteService.cambiarEstado(id, {estado: !estado})
  .then(() => {
    const i = clientes.value.findIndex(cliente => cliente.id === id);
    clientes.value[i].estado = !estado;
  })
  .catch(error => console.log(error));
}

const eliminarCliente = (id) => {
  ClienteService.eliminarCliente(id)
  .then(() => {
    clientes.value = clientes.value.filter(cliente => cliente.id !== id);
  })
  .catch(error => console.log(error));
}

</script>
