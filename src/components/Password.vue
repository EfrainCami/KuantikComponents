<template>
    <div class="flex flex-col items-center bg-white rounded-xl p-4 w-fit">
        <div>
            <img class="h-14" src="../assets/robot_saludo.de0acfb2.png" alt="">
        </div>
        <div class="flex items-center m-6 space-x-4">
            <img class="h-14" src="../assets/robot_saludo.de0acfb2.png" alt="">
            <h1 class="font-semibold text-2xl">Reestablezca su contraseña</h1>
        </div>
        <div class="flex space-x-3">
            <input v-model="inputPassword" class="w-80 p-2 border-2 rounded-md border-green-400"
                :type="passwordVisible" name="" id="" placeholder="contraseña" maxlength="20">
            <img class="cursor-pointer" @click="showPassword" :src="eyeIcon" alt="Mostrar contraseña">

        </div>

        <div class="flex mt-2">
            <p>Seguridad nivel: </p>
            <p>FUERTE</p>
            <img src="../assets/shield-lock-outline.svg" alt="Security level">
        </div>
        <p class="mt-4">La contraseña debe contener:</p>
        <div class="flex flex-col items-start">
            <div class="flex justify-center items-center">
                <img :class="{ 'hidden': !checkEightCharacters }" class="h-4" src="../assets/heavy-check-mark.svg" alt="">
                <p>8 caracteres como mínimo</p>
            </div>
            <div class="flex justify-center items-center">
                <img :class="{ 'hidden': !checkLowerCase }" class="h-4" src="../assets/heavy-check-mark.svg" alt="">
                <p>Minúsculas</p>
            </div>
            <div class="flex justify-center items-center">
                <img :class="{ 'hidden': !checkUpperCase }" class="h-4" src="../assets/heavy-check-mark.svg" alt="">
                <p>Mayúsculas</p>
            </div>
            <div class="flex justify-center items-center">
                <img :class="{ 'hidden': !checkSpecialCharacters }" class="h-4" src="../assets/heavy-check-mark.svg" alt="">
                <p>Símbolos (!$#%)</p>
            </div>
            <div class="flex justify-center items-center">
                <img :class="{ 'hidden': !checkNumbers }" class="h-4" src="../assets/heavy-check-mark.svg" alt="">
                <p>Números</p>
            </div>
        </div>
        <div class="flex items-start">
            <p class="text-red-600">*hola</p>
        </div>
        <button @click="submit" class="flex mb-6 w-fit p-5 bg-blue-900 rounded-full text-white">
            <img class="invert mr-3" src="../assets/arrow-right-1.svg" alt="">
            Reestablecer contraseña
        </button>
    </div>
</template>

<script setup>
import { ref, reactive, onUpdated } from 'vue';
import { useVuelidate } from '@vuelidate/core'
import { required, minLength, maxLength } from '@vuelidate/validators'

const inputPassword = ref("")
const passwordVisible = ref("password")
const eyeIcon = ref("/src/assets/eye-password-hide.svg")

const checkEightCharacters = ref(false)
const checkLowerCase = ref(false)
const checkUpperCase = ref(false)
const checkSpecialCharacters = ref(false)
const checkNumbers = ref(false)

const showPassword = () => {
    if (passwordVisible.value === "password") {
        passwordVisible.value = "text"
        eyeIcon.value = "/src/assets/eye-password-show.svg"
    } else {
        passwordVisible.value = "password"
        eyeIcon.value = "/src/assets/eye-password-hide.svg"
    }
}

const hasLowerCase = (str) => {
    return str.toUpperCase() != str;
}

const hasUpperCase = (str) => {
    return /[A-Z]/.test(str);
}

const hasSpecialCharacters = (str) => {
    return /[ `!@#$%^&*()_+\-=\[\]{};':"\\|,.<>\/?~]/.test(str)
}

const hasNumbers = (str) => {
    return /\d/.test(str);
}

const hasEightCharacters = (str) => {
    return str.length > 7
}

const checkPassword = (str) => {
    checkEightCharacters.value = hasEightCharacters(str)
    checkLowerCase.value = hasLowerCase(str)
    checkUpperCase.value = hasUpperCase(str)
    checkNumbers.value = hasNumbers(str)
    checkSpecialCharacters.value = hasSpecialCharacters(str)
}

const formData = reactive({
    inputPasswordForm: inputPassword.value
});

const rules = {
    inputPasswordForm: {
        required, 
        minLength: minLength(8),
        maxLength: maxLength(20),
        hasLowerCase,
        hasNumbers,
        hasUpperCase,
        hasSpecialCharacters
    }
}

const v$ = useVuelidate(rules, formData)

console.log(hasUpperCase("AAAA"))

onUpdated(() => {
    checkPassword(inputPassword.value)
})

const submit = async () => {
    const result = await v$.value.$validate();
    if (result) {
        alert("Contraseña correcta")
    } else {
        alert("Contraseña incorrecta")
    }
}
</script>