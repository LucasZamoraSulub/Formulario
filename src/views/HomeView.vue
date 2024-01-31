<script setup lang="ts">
import { ref } from 'vue'
import type { Ref } from 'vue'

const name: Ref<string> = ref('')
const lastName: Ref<string> = ref('')
const age: Ref<number> = ref(1)
const selectedGen: Ref<string> = ref('masculino')
const customGen: Ref<string> = ref('')

const errors = ref({
  name: '',
  lastName: '',
  age: '',
  otherGen: ''
})

// Validacion del nombre
const validateName = () => {
  validateLastName()
  if (name.value.length < 5 || name.value.length > 18) {
    errors.value.name = 'El nombre debe tener entre 5 y 18 caracteres.'
  } else {
    errors.value.name = ''
  }
}

// Validacion del apellido
const validateLastName = () => {
  if (lastName.value === name.value) {
    errors.value.lastName = 'El apellido no puede ser igual al nombre.'
  } else {
    errors.value.lastName = ''
  }
}

// Validacion de la edad
const validateAge = () => {
  if (age.value <= 0 || age.value > 60) {
    errors.value.age = 'La edad debe ser mayor a 0 y menor a 60.'
  } else {
    errors.value.age = ''
  }
}

// Validacion del genero si se selecciona "otro"
const validateCustomGen = () => {
  if (selectedGen.value === 'otro' && customGen.value.trim() === '') {
    errors.value.otherGen = 'Por favor, ingresa un valor para el campo "Agregar Género".'
  } else {
    errors.value.otherGen = ''
  }
}

// Mensaje en consola al momento de presionar el boton de "enviar"
const submitForm = () => {
  console.log('Formulario enviado exitosamente :D')
}
</script>

<template>
  <div class="container">
    <div>
      <h1 class="title">Formulario</h1>
    </div>

    <form @submit.prevent="submitForm">
      <!-- campo nombre -->
      <div class="form-camp">
        <label for="name">Nombre:</label>
        <input v-model="name" @input="validateName" type="text" id="name" />
        <p v-if="errors.name">{{ errors.name }}</p>
      </div>

      <!-- campo apellido -->
      <div class="form-camp">
        <label for="lastName">Apellido:</label>
        <input v-model="lastName" @input="validateLastName" type="text" id="lastName" />
        <p v-if="errors.lastName">{{ errors.lastName }}</p>
      </div>

      <!-- campo edad -->
      <div class="form-camp">
        <label for="age">Edad:</label>
        <input v-model.number="age" @input="validateAge" type="number" id="age" />
        <p v-if="errors.age">{{ errors.age }}</p>
      </div>

      <!-- campo genero -->
      <div class="form-camp">
        <label for="Gen">Género:</label>
        <select v-model="selectedGen" id="Gen">
          <option value="masculino">Masculino</option>
          <option value="femenino">Femenino</option>
          <option value="otro">Otro</option>
        </select>
      </div>

      <!-- campo de genero, si se selecciono la opcion de "otro" -->
      <div class="form-camp" v-if="selectedGen === 'otro'">
        <label for="customGen">Agregar Género:</label>
        <input v-model="customGen" type="text" @input="validateCustomGen" id="customGen" />
        <p v-if="errors.otherGen">{{ errors.otherGen }}</p>
      </div>

      <!-- boton de enviar que funciona unicamente si los campos no estan vacios y las validaciones son correctas -->
      <button
        :disabled="
          !(
            name.trim() !== '' &&
            lastName.trim() !== '' &&
            errors.name === '' &&
            errors.lastName === '' &&
            errors.age === '' &&
            (selectedGen !== 'otro' ||
              (selectedGen === 'otro' && customGen.trim() !== '' && errors.otherGen === ''))
          )
        "
        type="submit"
        class="button"
      >
        Enviar
      </button>
    </form>
  </div>
</template>

<style scoped>
/* Estilos del contenedor */
.title {
  color: white;
  text-align: center;
}

.container {
  margin: 0 auto;
  width: 300px;
  background-color: rgb(3, 21, 24);
  border-radius: 15px;
  padding: 20px;
  box-shadow: 0px 0px 10px rgba(22, 189, 211, 0.5);
}

form {
  display: flex;
  flex-direction: column;
}

.form-camp {
  display: flex;
  flex-direction: column;
  margin-bottom: 5px;
}

label {
  margin-bottom: 5px;
  color: white;
}

p {
  color: rgb(243, 226, 226);
  font-size: 12px;
  font-weight: bold;
}

input,
select {
  padding: 8px;
  margin-bottom: 10px;
  border: 1px solid #909191;
  border-radius: 5px;
  background-color: rgb(11, 95, 95);
  color: white;
}

/* Estilos del boton */
.button {
  margin-top: 10px;
  background-color: #034646;
  color: white;
  font-weight: bold;
  border-radius: 5px;
  padding: 10px 15px;
  cursor: pointer;
  border: 1px solid #0a3636;
}

.button:hover {
  background-color: #0d7c7c;
}

.button:disabled {
  background-color: rgb(102, 102, 102);
  color: rgb(175, 159, 159);
  font-weight: bold;
  border: 1px solid #666;
  cursor: not-allowed;
}
</style>
