<script setup>
    import { reactive } from 'vue'
    import Alerta from './Alerta.vue'

    const alerta = reactive({
        tipo: '',
        mensaje: ''
    })

    /* ↓ Qué hace: Agrega un nuevo evento para notificar cuando se debe guardar el paciente. Beneficio: Separa la lógica de validación (en Formulario) de la lógica de guardado (en App). */
    const emit = defineEmits(['update:nombre', 'update:propietario', 'update:email', 'update:alta', 'update:sintomas', 'guardar-paciente']) 
    // ↑ Declara que el componente Formulario pueda emitir un evento llamado update:nombre
    /* Esto permite la comunicación del componente hijo (Formulario) hacia el padre (App.vue), enviando cambios del input hacia arriba. */
    /* Qué hace: Declara todos los eventos que el componente puede emitir.
       Beneficio: Ahora TODOS los campos del formulario tienen comunicación bidireccional con App.vue, no solo el nombre. */

    const props = defineProps({ 
    /* ↑ ↓ Qué hace: Define todas las propiedades que el componente recibe del padre.
    Beneficio: El formulario ahora puede recibir y mostrar datos completos del paciente. */
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
     * ↑ Props del componente Formulario
     * 
     * @property {String} nombre - Nombre del paciente (requerido)
     * Este prop es obligatorio y debe ser de tipo String. Se utiliza para
     * identificar al paciente en el formulario de registro veterinario.
     */


    const validar = () => {
        if(Object.values(props).includes('')){ /* ← Qué hace: Valida que todas las props estén llenas antes de enviar. Beneficio: La validación ahora funciona correctamente con el sistema de props */
            alerta.mensaje = 'Todos los campos son obligatorios'
            alerta.tipo = 'error'
            return
        }

        emit('guardar-paciente')
        /* ↑ Qué hace: Si la validación es exitosa, emite el evento guardar-paciente. Beneficio: El componente hijo notifica al padre que los datos son válidos y están listos para guardarse. */

        alerta.mensaje = 'Paciente Almacenado Correctamente'
        alerta.tipo = 'exito'

        setTimeout(() => {
            Object.assign(alerta, {
                tipo: '',
                mensaje: ''
            })
        }, 3000)

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
                /><!-- Qué hace: Vincula cada campo con su respectiva prop y emite cambios.
                       Beneficio: Cualquier cambio en cualquier campo se sincroniza automáticamente con App.vue. -->
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
            <!-- Flujo completo de Datos: 
                Usuario escribe en input
                ↓
                2. @input emite 'update:nombre' con el nuevo valor
                ↓
                3. v-model:nombre en App.vue recibe el valor
                ↓
                4. paciente.nombre se actualiza
                ↓
                5. :value="nombre" refleja el cambio en el input
            -->

            <!-- Botón para Registrar Paciente -->
            <input
                type="submit"
                class="bg-indigo-600 w-full p-3 text-white uppercase font-bold hover:bg-indigo-700 cursor-pointer transition-colors"
                value="Registrar Paciente"
            >
            <!-- Flujo de Guardado: 
            1. Usuario hace clic en "Registrar Paciente"
            ↓
            2. Se ejecuta validar() en Formulario
            ↓
            3. Si es válido → emit('guardar-paciente')
            ↓
            4. App.vue escucha y ejecuta guardarPaciente()
            ↓
            5. paciente se agrega al array pacientes
            -->

        </form>
    </div>
</template>
