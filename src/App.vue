<script setup>
  import { ref, reactive } from 'vue'
  import Header from './components/Header.vue';
  import Formulario from './components/Formulario.vue';
  import Paciente from './components/Paciente.vue';
  
  const pacientes = ref ([])

  const paciente = reactive({
      nombre: '',
      propietario: '',
      email: '',
      alta: '',
      sintomas: ''
  })

  const guardarPaciente = () => {
    pacientes.value.push(paciente)
  }
  /* ↑ Qué hace: Agrega el paciente actual al array de pacientes. 
  Beneficio: Ahora puedes almacenar múltiples pacientes en tu aplicación. */

</script>

<template>
  <div class="container mx-auto mt-20">
    <Header/>

    <div class="mt-12 md:flex">
      <Formulario
          v-model:nombre="paciente.nombre"
          v-model:propietario="paciente.propietario"
          v-model:email="paciente.email"
          v-model:alta="paciente.alta"
          v-model:sintomas="paciente.sintomas"
          @guardar-paciente="guardarPaciente"
      /><!-- ↑ En App.vue usas: v-model:nombre="paciente.nombre"
        En Formulario.vue recibes la prop nombre y emites update:nombre
        Esto crea una sincronización bidireccional automática -->
        <!-- Qué hace: Conecta el evento emitido por Formulario con la función en App. Beneficio: Cuando el usuario hace clic en "Registrar Paciente", se ejecuta la lógica de guardado. -->

      <div class="md:w-1/2 md:h-screen overflow-y-scroll">
        <h3 class="font-black text-3xl text-center">Administra a tus Pacientes</h3>

        <div v-if="pacientes.length > 0">
          <p class="text-lg mt-5 text-center mb-10">
            Información de 
            <span class="text-indigo-600 font-bold">Pacientes</span>
          </p>

          <Paciente
            v-for="paciente in pacientes"
            :paciente="paciente"
          />
        </div>
        <p v-else class="mt-10 text-2xl text-center">No hay pacientes</p>

      </div>
    </div>
  </div>
</template>

