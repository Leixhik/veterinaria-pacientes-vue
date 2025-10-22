<script setup>
    import { reactive } from 'vue'
    import Alerta from './Alerta.vue'

    const alerta = reactive({
        tipo: '',
        mensaje: ''
    })

    defineEmits(['update:nombre', 'update:propietario', 'update:email', 'update:alta', 'update:sintomas']) 
    // Declara que el componente Formulario pueda emitir un evento llamado update:nombre
    /* Esto permite la comunicación del componente hijo (Formulario) hacia el padre (App.vue), enviando cambios del input hacia arriba. */

    const props = defineProps({
        nombre: {
            type: String,
            required: true
        },
        propietario: {
            type: String,
            required: true
        },
        email: {
            type: String,
            required: true
        },
        alta: {
            type: String,
            required: true
        },
        sintomas: {
            type: String,
            required: true
        }
    })
    /**
     * Props del componente Formulario
     * 
     * @property {String} nombre - Nombre del paciente (requerido)
     * Este prop es obligatorio y debe ser de tipo String. Se utiliza para
     * identificar al paciente en el formulario de registro veterinario.
     */


    const validar = () => {
        if(Object.values(props).includes('')){
            alerta.mensaje = 'Todos los campos son obligatorios'
            alerta.tipo = 'error'
            return
        }

    }

</script>

<template>
    <div class="md:w-1/2">
        <h2 class="font-black text-3xl text-center">Seguimiento Pacientes</h2>

        <p class="text-lg mt-5 text-center mb-10">
            Añade Pacientes y
            <span class="text-indigo-600 font-bold">Adminístalos</span>
        </p>

        <Alerta
            v-if="alerta.mensaje"
            :alerta="alerta"
        />

        <form
           class="bg-white shadow-md rounded-lg py-10 px-5 mb-10"
           @submit.prevent="validar"
        >

            <!-- Campo para Datos Perro -->
            <div class="mb-5">

                <label
                    for="mascota"
                    class="block text-gray-700 uppercase font-bold"
                >
                    Nombre Mascota
                </label>

                <input
                    id="mascota"
                    type="text"
                    placeholder="Nombre de la Mascota"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    :value="nombre"
                    @input="$emit('update:nombre', $event.target.value)"
                />
            </div>

            <!-- Campo para Datos Dueño -->
            <div>
                <label
                    for="propietario"
                    class="block text-gray-700 uppercase font-bold"
                >
                    Nombre Propietario
                </label>

                <input
                    id="mascota"
                    type="text"
                    placeholder="Nombre del Propietario"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    :value="propietario"
                    @input="$emit('update:propietario', $event.target.value)"
                />
            </div>

            <!-- Campo para Email de Contacto-->
            <div>
                <label
                    for="email"
                    class="block text-gray-700 uppercase font-bold"
                >
                    Email
                </label>

                <input
                    id="email"
                    type="email"
                    placeholder="Email del Propietario"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    :value="email"
                    @input="$emit('update:email', $event.target.value)"
                />
            </div>

            <!-- Campo para Alta de Paciente-->
            <div>
                <label
                    for="alta"
                    class="block text-gray-700 uppercase font-bold"
                >
                    Fecha de Alta
                </label>

                <input
                    id="alta"
                    type="date"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    :value="alta"
                    @input="$emit('update:alta', $event.target.value)"
                />
            </div>

            <!-- Campo para Síntomas-->
            <div>
                <label
                    for="sintomas"
                    class="block text-gray-700 uppercase font-bold"
                >
                    Fecha de Alta
                </label>

                <input
                    id="sintomas"
                    placeholder="Describe los síntomas del paciente:"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md h-40"
                    :value="sintomas"
                    @input="$emit('update:sintomas', $event.target.value)"
                />
            </div>

            <!-- Botón para Registrar Paciente -->
            <input
                type="submit"
                class="bg-indigo-600 w-full p-3 text-white uppercase font-bold hover:bg-indigo-700 cursor-pointer transition-colors"
                value="Registrar Paciente"
            >

        </form>
    </div>
</template>
