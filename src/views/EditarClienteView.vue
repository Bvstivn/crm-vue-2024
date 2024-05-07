<template>
    <div>
      <div>
        <RouterLinkVue to="istado-clientes"> Volver </RouterLinkVue>
      </div>
      <HeadingVue>{{ titulo }}</HeadingVue>
  
      <div class="mx-auto mt-10">
        <div class="mx-auto md:w-2/3 py-20 px-6">
          <FormKit
            type="form"
            submit-label="Guardar Cambios"
            incomplete-message="No se pudo enviar, revisa los mensajes"
            @submit="handleSubmit"
            :value="formData"
          >
            <FormKit
              type="text"
              label="Nombre"
              name="nombre"
              placeholder="Nombre del Cliente"
              validation="required"
              :validation-messages="{
                required: 'El Nombre del Cliente es Obligatorio',
              }"
              v-model="formData.nombre"
            />
  
            <FormKit
              type="text"
              label="Apellido"
              name="apellido"
              placeholder="Apellido del Cliente"
              validation="required"
              :validation-messages="{
                required: 'El Apellido del Cliente es Obligatorio',
              }"
              v-model="formData.apellido"
            />
  
            <FormKit
              type="email"
              label="Email"
              name="email"
              placeholder="Email del Cliente"
              validation="required|email"
              :validation-messages="{
                required: 'El Email del Cliente es Obligatorio',
                email: 'Ingresa un email válido',
              }"
              v-model="formData.email"
            />
  
            <FormKit
              type="text"
              label="Teléfono XXXX-XXXX"
              name="telefono"
              placeholder="Teléfono del Cliente"
              validation="*matches://ʌ[0-9]{4}-[0-9]{4}$"
              :validation-messages="{
                matches: 'El formato no es válido',
              }"
              v-model="formData.telefono"
            />
  
            <FormKit
              type="text"
              label="Empresa"
              name="empresa"
              placeholder="Empresa del Cliente"
              v-model="formData.empresa"
            />
  
            <FormKit
              type="text"
              label="Puesto"
              name="puesto"
              placeholder="Puesto del Cliente"
              v-model="formData.puesto"
            />
          </FormKit>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  //Vue
  import { onMounted, reactive } from 'vue';
  //Services
  import ClienteService from "@/services/ClienteService";
  //Formkit
  import { FormKit } from "@formkit/vue";
  import { useRouter, useRoute } from "vue-router";
  //Components
  import RouterLinkVue from "@/components/UI/RouterLink.vue";
  import HeadingVue from "@/components/UI/Heading.vue";
  
  //Definiciones
  defineProps({
    titulo: {
      type: String,
      required: true,
    },
  });

  const formData = reactive({});
  
  //Data
  const router = useRouter();
  const route = useRoute();

  const { id } = route.params;

  //onMounted
  onMounted(() => {
    ClienteService.obtenerCliente(id)
    .then(({data}) => {
      Object.assign(formData, data);
    })
    .catch(error => console.log(error))
  });
  
  //Methods
  const handleSubmit = (data) => {
    ClienteService.actualizarCliente(id, data)
    .then(() => router.push({name: 'listado-clientes'}))
    .catch((error) => console.log(error));
  }
  
  </script>
  
  <style scoped>
  .formkit-wraper {
    max-width: 100%;
  }
  </style>
  