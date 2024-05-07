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
          submit-label="Agregar Cliente"
          incomplete-message="No se pudo enviar, revisa los mensajes"
          @submit="handleSubmit"
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
          />

          <FormKit
            type="text"
            label="Empresa"
            name="empresa"
            placeholder="Empresa del Cliente"
          />

          <FormKit
            type="text"
            label="Puesto"
            name="puesto"
            placeholder="Puesto del Cliente"
          />
        </FormKit>
      </div>
    </div>
  </div>
</template>

<script setup>
//Services
import ClienteService from "@/services/ClienteService";
//Formkit
import { FormKit } from "@formkit/vue";
import { useRouter } from "vue-router";
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

//Data
const router = useRouter();

//Methods
const handleSubmit = (data) => {
  data.estado = 1;
  ClienteService.agregarCliente(data)
  .then(respuesta => {
    //Redireccionar
    router.push('/');
  })
  .catch(error => console.log(error))
}

</script>

<style scoped>
.formkit-wraper {
  max-width: 100%;
}
</style>
