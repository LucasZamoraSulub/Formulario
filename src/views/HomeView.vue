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

const validateName = () => {
  validateLastName()
  if (name.value.length < 5 || name.value.length > 18) {
    errors.value.name = 'El nombre debe tener entre 5 y 18 caracteres.'
  } else {
    errors.value.name = ''
  }
}

const validateLastName = () => {
  if (lastName.value === name.value) {
    errors.value.lastName = 'El apellido no puede ser igual al nombre.'
  } else {
    errors.value.lastName = ''
  }
}

const validateAge = () => {
  if (age.value <= 0 || age.value > 60) {
    errors.value.age = 'La edad debe ser mayor a 0 y menor a 60.'
  } else {
    errors.value.age = ''
  }
}
</script>

<template>
  <div class="about">
    <h1 class="title">Formulario</h1>
  </div>

  <div class="container">
    <form>
      <div class="form-camp">
        <label for="name">Nombre:</label>
        <input v-model="name" @input="validateName" type="text"/>
        <p v-if="errors.name">{{ errors.name }}</p>
      </div>

      <div class="form-camp">
        <label for="lastName">Apellido:</label>
        <input v-model="lastName" @input="validateLastName" type="text"/>
        <p v-if="errors.lastName">{{ errors.lastName }}</p>
      </div>

      <div class="form-camp">
        <label for="age">Edad:</label>
        <input v-model.number="age" @input="validateAge" type="number"/>
        <p v-if="errors.age">{{ errors.age }}</p>
      </div>

      <div class="form-camp">
        <label for="Gen">Género:</label>
        <select v-model="selectedGen">
          <option value="masculino">Masculino</option>
          <option value="femenino">Femenino</option>
          <option value="otro">Otro</option>
        </select>
      </div>

      <div v-if="selectedGen === 'otro'">
        <label for="customGen">Agregar Género:</label>
        <input v-model="customGen" type="text" id="customGen" />
        <p v-if="errors.otherGen">{{ errors.otherGen }}</p>
      </div>
    </form>
  </div>
</template>

<style scoped>
.title{
  color: white;
  text-align: center;
}
.container {
  width: 300px;
  margin: 0 auto;
  background-color: rgb(75, 73, 73);
  border-radius: 15px;
  padding: 20px;
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
  color: red;
  font-size: 12px;
  font-weight: bold;

}
input,
select {
  padding: 8px;
  margin-bottom: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
}
</style>
