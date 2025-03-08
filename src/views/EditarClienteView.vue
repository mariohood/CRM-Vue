<script setup>
  import { onMounted, reactive } from 'vue'
  import { FormKit } from '@formkit/vue'
  import { useRouter, useRoute } from 'vue-router'
  import ClienteService from '../services/ClienteService'
  import RouterLink from '@/components/UI/RouterLink.vue'
  import Heading from '@/components/UI/Heading.vue'

const router = useRouter()
const route = useRoute()

const { id } = route.params

const formData = reactive ({})

onMounted(() => {
  ClienteService.obtenerCliente(id)
  .then(({data}) => {
    Object.assign(formData, data)
  })
  .catch(error => console.log(error))
})

  defineProps({
    titulo: {
      type: String
    }
  })

  const handleSubmit = (data) => {
    ClienteService.actualizarCliente(id, data)
    .then(() => router.push({name: 'listado-clientes'}))
    .catch(error => console.log(error))
  }
</script>

<template>
  <div>
    <div class="flex justify-end">
      <RouterLink 
        to="listado-clientes"
        >
        Volver
      </RouterLink>
    </div>

    <Heading>{{ titulo }}</Heading>

    <div class="mx-auto mt-10 bg-white shadow">
      <div class="mx-auto md:w-2/3 py-20 px-6">
      <FormKit
        type="form"
        submit-label="Guardar Cambios"
        incomplete-message="No se pudo enviar. Por favor completa todos los campos"
        @submit="handleSubmit"
        :value="formData"
      >
        <FormKit 
          type="text"
          label="Nombre"
          name="nombre"
          placeholder="Nombre del cliente"
          validation="required"
          :validation-messages="{ required: 'El Nombre del cliente es Obligatorio' }"
          v-model="formData.nombre"
        />

        <FormKit 
          type="text"
          label="Apellido"
          name="apellido"
          placeholder="Apellido del cliente"
          validation="required"
          :validation-messages="{ required: 'El Apellido del cliente es Obligatorio' }"
          v-model="formData.apellido"
        />

        <FormKit 
          type="email"
          label="Email"
          name="email"
          placeholder="Email del cliente"
          validation="required | email"
          :validation-messages="{ required: 'El Email del cliente es Obligatorio', email: 'Coloca un Email válido' }"
          v-model="formData.email"
        />

        <FormKit 
          type="text"
          label="Teléfono"
          name="telefono"
          placeholder="Teléfono: (xx)xxx-xxx-xxxx"
          validation="*matches:/^\(\d{2}\)\d{3}-\d{3}-\d{3}$/"
          :validation-messages="{ matches: 'El Formato no es válido' }"
          v-model="formData.telefono"
        />

        <FormKit 
          type="text"
          label="Empresa"
          name="empresa"
          placeholder="Empresa del cliente"
          v-model="formData.empresa"
        />

        <FormKit 
          type="text"
          label="Puesto"
          name="puesto"
          placeholder="Puesto del cliente"
          v-model="formData.puesto"
        />

      </FormKit>
      </div>
    </div>
    
  </div>
</template>

<style>
  .formkit-wrapper {
    max-width: 100%;
  }
</style>


