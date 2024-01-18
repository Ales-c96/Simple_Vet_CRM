<script setup>
import { ref, reactive, onMounted, watch } from 'vue'
import { uid } from 'uid'
import Header from "./components/Header.vue"
import Form from "./components/Form.vue"
import Patient from "./components/Patient.vue"

//Array with the patients saved
const patients = ref([])

//Object single patient
const patient = reactive({
  id: null,
  petName: '',
  ownerName: '',
  ownerEmail: '',
  dischargeDate: '',
  petSymptoms: ''

})

//Every time a patients is added or opdated or deleted is added or deleted from local storage
watch(patients, () => {
  savePatientLocalStorage()
}, {
  deep: true
})

onMounted(() => {
  //Get and set the patients from local storage
  const storagePatients = localStorage.getItem('patients')
  if (storagePatients) {
    patients.value = JSON.parse(storagePatients)
  }
})

const savePatientLocalStorage = () => {
  localStorage.setItem('patients', JSON.stringify(patients.value))
}

const savePatient = () => {
  //Saves the patient
  if (patient.id) {
    const { id } = patient
    const index = patients.value.findIndex((patientState) => patientState.id === id)
    patients.value[index] = { ...patient }
  } else {
    patients.value.push({
      ...patient,
      id: uid()
    })
  }

  //Reload de patient object
  Object.assign(patient, {
    id: null,
    petName: '',
    ownerName: '',
    ownerEmail: '',
    dischargeDate: '',
    petSymptoms: ''
  })
}

const updatePatient = (id) => {
  const editPatient = patients.value.filter(patient => patient.id === id)[0]
  Object.assign(patient, editPatient)
}

const deletePatient = (id) => {
  patients.value = patients.value.filter(patient => patient.id !== id)

}

const getPatients = () => {

}

</script>

<template>
  <div class="container mx-auto mt-20">
    <Header />
    <div class="mt-12 md:flex">
      <Form v-model:petName="patient.petName" v-model:ownerName="patient.ownerName"
        v-model:ownerEmail="patient.ownerEmail" v-model:dischargeDate="patient.dischargeDate"
        v-model:petSymptoms="patient.petSymptoms" @save-patient="savePatient" :id="patient.id" />

      <div class="md:w-1/2 md:h-screen overflow-y-scroll">
        <h3 class="font-black text-3xl text-center">
          Administra tus pacientes
        </h3>
        <div v-if="patients.length > 0" class="">
          <p class="text-lg mt-5 text-center mb-10">
            Información de
            <span class="text-indigo-600 font-bold">Pacientes</span>
          </p>
          <Patient @update-patient="updatePatient" @delete-patient="deletePatient" v-for="patient in patients"
            :patient="patient" />
        </div>
        <p v-else class="mt-20 text-2xl text-center">No Hay Pacientes</p>
      </div>
    </div>
  </div>
</template>
