<script setup>
import { reactive } from 'vue'
import Alert from './Alert.vue'

const alert = reactive({
    type: '',
    msg: ''
})

const emit = defineEmits(['update:petName', 'update:ownerName', 'update:ownerEmail', 'update:dischargeDate', 'update:petSymptoms', 'save-patient'])
const props = defineProps({
    petName: {
        type: String,
        requiered: true
    },
    ownerName: {
        type: String,
        requiered: true
    },
    ownerEmail: {
        type: String,
        requiered: true
    },
    dischargeDate: {
        type: String,
        requiered: true
    },
    petSymptoms: {
        type: String,
        requiered: true
    }
})
const validateForm = () => {
    if (Object.values(props).includes('')) {
        alert.msg = 'Todos los campos son obligatorios.'
        alert.type = 'error'
        return
    }

    emit('save-patient')
}

</script>

<template>
    <div class="md:w-1/2">
        <h2 class="font-black text-3xl text-center">Seguimiento Pacientes</h2>
        <p class="text-lg mt-5 text-center mb-10">
            Añade pacientes y
            <span class="text-indigo-600 font-bold">Adminístralos</span>
        </p>
        <Alert v-if="alert.msg" :alert="alert" />
        <form @submit.prevent="validateForm" class="bg-white shadow-md rounded-lg py-10 px-5 mb-10">
            <div class="mb-5">
                <label for="pet" class="block text-gray-700 uppercase font-bold">
                    Nombre mascota
                </label>
                <input type="text" name="pet" id="pet" placeholder="Nombre de la mascota"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md" :value="petName"
                    @input="$emit('update:petName', $event.target.value)">
            </div>
            <div class="mb-5">
                <label for="owner" class="block text-gray-700 uppercase font-bold">
                    Nombre propietario
                </label>
                <input type="text" name="owner" id="owner" placeholder="Nombre del propietario"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md" :value="ownerName"
                    @input="$emit('update:ownerName', $event.target.value)">
            </div>
            <div class="mb-5">
                <label for="email" class="block text-gray-700 uppercase font-bold">
                    Correo electrónico
                </label>
                <input type="email" name="email" id="email" placeholder="Email del propietario"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md" :value="ownerEmail"
                    @input="$emit('update:ownerEmail', $event.target.value)">
            </div>
            <div class="mb-5">
                <label for="discharge" class="block text-gray-700 uppercase font-bold">
                    Fecha de alta
                </label>
                <input type="date" name="discharge" id="discharge" placeholder="Fecha de alta"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md" :value="dischargeDate"
                    @input="$emit('update:dischargeDate', $event.target.value)">
            </div>
            <div class="mb-5">
                <label for="symptoms" class="block text-gray-700 uppercase font-bold">
                    Síntomas
                </label>
                <textarea id="symptoms" placeholder="Describe los síntomas"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md h-40" :value="petSymptoms"
                    @input="$emit('update:petSymptoms', $event.target.value)">
                </textarea>
            </div>

            <input type="submit" name="" id=""
                class="bg-indigo-600 w-full p-3 text-white uppercase font-bold hover:bg-indigo-700 cursor-pointer transition-colors"
                value="Registrar paciente">
        </form>
    </div>
</template>
