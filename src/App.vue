<script setup>
import { ref, reactive } from 'vue'
import Header from "./components/Header.vue"
import Form from "./components/Form.vue"
import Patient from "./components/Patient.vue"

const patients = ref([])

const patient = reactive({
  petName: '',
  ownerName: '',
  ownerEmail: '',
  dischargeDate: '',
  petSymptoms: ''

})

const savePatient = () => {
  patients.value.push(patient)
}

</script>

<template>
  <div class="container mx-auto mt-20">
    <Header />

    <div class="mt-12 md:flex">

      <Form v-model:petName="patient.petName" v-model:ownerName="patient.ownerName"
        v-model:ownerEmail="patient.ownerEmail" v-model:dischargeDate="patient.dischargeDate"
        v-model:petSymptoms="patient.petSymptoms" @save-patient="savePatient" />

      <div class="md:w-1/2 md:h-screen overflow-y-scroll">
        <h3 class="font-black text-3xl text-center">
          Administra tus pacientes
        </h3>
        <div v-if="patients.length > 0" class="">
          <p class="text-lg mt-5 text-center mb-10">
            Información de
            <span class="text-indigo-600 font-bold">Pacientes</span>
          </p>
          <Patient v-for="patient in patients" :patient="patient" />
        </div>
        <p v-else class="mt-20 text-2xl text-center">No Hay Pacientes</p>
      </div>

    </div>
  </div>
</template>
