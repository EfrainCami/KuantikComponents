<template>
    <div class="flex h-fit rounded-3xl overflow-hidden bg-white w-1/2 2xl:w-auto">
        <div class="hidden 2xl:block">
            <img class="h-full object-cover" src="../assets/back.jpg" alt="Kuantik" />
        </div>
        <div class="flex flex-col w-full 2xl:w-2/3 items-center justify-center">
            <div class="flex flex-col items-center space-y-4 p-4">
                <div class="flex items-center space-x-4">
                    <h1 class="font-semibold text-4xl">Restablezca su contraseña</h1>
                </div>
                <div class="flex">
                    <div class="flex flex-col space-y-3">
                        <div class="flex items-center">
                            <input v-model="formData.inputPasswordForm"
                                class="w-80 h-10 p-2 border-2 rounded-md border-green-400" :type="passwordVisible" name=""
                                id="" placeholder="Contraseña" maxlength="20" />
                        </div>
                        <div class="flex flex-col items-center">
                            <input v-model="formData.inputRepeatPasswordForm"
                                class="w-80 h-10 p-2 border-2 rounded-md border-green-400" :type="passwordVisible" name=""
                                id="" placeholder="Repetir contraseña" maxlength="20" />
                                <p class="text-red-500" v-if="v$.inputRepeatPasswordForm.$error">* Las contraseñas no coinciden</p>
                        </div>
                    </div>
                    <div class="flex items-center ml-6">
                        <img class="cursor-pointer h-8" @click="showPassword" :src="eyeIcon" alt="Mostrar contraseña" />
                    </div>
                </div>

                <div class="flex items-center transition">
                    <p class="text-lg">Nivel de seguridad:</p>
                    <p :class="{
                        'text-red-500': securityWeak,
                        'text-yellow-500': securityMedium,
                        'text-green-500': securityStrong,
                    }" class="text-lg transition">
                        {{ securityLevelDescription }}
                    </p>
                    <svg class="transition" :class="{
                        'fill-red-500': securityWeak,
                        'fill-yellow-500': securityMedium,
                        'fill-green-500': securityStrong,
                    }" height="24" viewBox="0 0 24 24" width="24" xmlns="http://www.w3.org/2000/svg">
                        <path
                            d="m21 11c0 5.55-3.84 10.74-9 12-5.16-1.26-9-6.45-9-12v-6l9-4 9 4zm-9 10c3.75-1 7-5.46 7-9.78v-4.92l-7-3.12-7 3.12v4.92c0 4.32 3.25 8.78 7 9.78m2.8-10v-1.5c0-1.4-1.4-2.5-2.8-2.5s-2.8 1.1-2.8 2.5v1.5c-.6 0-1.2.6-1.2 1.2v3.5c0 .7.6 1.3 1.2 1.3h5.5c.7 0 1.3-.6 1.3-1.2v-3.5c0-.7-.6-1.3-1.2-1.3m-1.3 0h-3v-1.5c0-.8.7-1.3 1.5-1.3s1.5.5 1.5 1.3z" />
                    </svg>
                </div>
            </div>

            <div class="flex flex-col w-full justify-center items-center bg-gray-200">
                <p class="my-4 text-xl font-semibold">La contraseña debe contener:</p>
                <div class="flex flex-col items-start text-xl space-y-2 2xl:space-y-4">
                    <div class="flex justify-center items-center">
                        <img class="h-4 mr-2"
                        :src="checkEightCharacters ? checkImage : crossImage" alt="" />
                        <p>8 caracteres como mínimo</p>
                    </div>
                    <div class="flex justify-center items-center">
                        <img class="h-4 mr-2" :src="checkLowerCase ? checkImage : crossImage"
                            alt="" />
                        <p>Minúsculas</p>
                    </div>
                    <div class="flex justify-center items-center">
                        <img class="h-4 mr-2" :src="checkUpperCase ? checkImage : crossImage"
                            alt="" />
                        <p>Mayúsculas</p>
                    </div>
                    <div class="flex justify-center items-center">
                        <img class="h-4 mr-2" :src="checkSpecialCharacters ? checkImage : crossImage"
                            src="../assets/heavy-check-mark.svg" alt="" />
                        <p>Símbolos (!$#%)</p>
                    </div>
                    <div class="flex justify-center items-center">
                        <img class="h-4 mr-2" :src="checkNumbers ? checkImage : crossImage"
                            alt="" />
                        <p>Números</p>
                    </div>
                </div>
                <button @click="submit" class="flex my-12 p-3 px-5 rounded-full text-white bg-blue-900">
                    <img class="invert mr-3" src="../assets/arrow-right-1.svg" alt="" />
                    Restablecer contraseña
                </button>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, reactive, onUpdated } from "vue";
import { useVuelidate } from "@vuelidate/core";
import { minLength, maxLength, helpers } from "@vuelidate/validators";

const passwordVisible = ref("password");
const eyeIcon = ref("/src/assets/eye-password-hide.svg");


const checkImage = ref("/src/assets/heavy-check-mark.svg")
const crossImage = ref("/src/assets/cross-mark.svg")

const checkEightCharacters = ref(false);
const checkLowerCase = ref(false);
const checkUpperCase = ref(false);
const checkSpecialCharacters = ref(false);
const checkNumbers = ref(false);

const showPassword = () => {
    if (passwordVisible.value === "password") {
        passwordVisible.value = "text";
        eyeIcon.value = "/src/assets/eye-password-show.svg";
    } else {
        passwordVisible.value = "password";
        eyeIcon.value = "/src/assets/eye-password-hide.svg";
    }
};

const hasLowerCase = (str) => {
    return str.toUpperCase() != str;
};

const hasUpperCase = (str) => {
    return /[A-Z]/.test(str);
};

const hasSpecialCharacters = (str) => {
    return /[ `!@#$%^&*()_+\-=\[\]{};':"\\|,.<>\/?~]/.test(str);
};

const hasNumbers = (str) => {
    return /\d/.test(str);
};

const hasEightCharacters = (str) => {
    return str.length > 7;
};

const confirmPassword = (value) => value === formData.inputPasswordForm;

const checkPassword = (str) => {
    checkSpecialCharacters.value = false
    checkEightCharacters.value = false
    checkLowerCase.value = false
    checkUpperCase.value = false
    checkNumbers.value = false

    let securityLevel = 0;
    if (hasEightCharacters(str)) {
        checkEightCharacters.value = true;
        securityLevel++;
    }
    if (hasLowerCase(str)) {
        checkLowerCase.value = true;
        securityLevel++;
    }
    if (hasUpperCase(str)) {
        checkUpperCase.value = true;
        securityLevel++;
    }
    if (hasNumbers(str)) {
        checkNumbers.value = true;
        securityLevel++;
    }
    if (hasSpecialCharacters(str)) {
        checkSpecialCharacters.value = true;
        securityLevel++;
    }
    return securityLevel;
};

let password = ""

const securityWeak = ref(false);
const securityMedium = ref(false);
const securityStrong = ref(false);
const securityLevelDescription = ref("NULO");

const checkSecurityLevel = (securityLevel) => {
    securityWeak.value = false;
    securityMedium.value = false;
    securityStrong.value = false;
    if (securityLevel == 0) {
        securityLevelDescription.value = "NULO";
    } else if (securityLevel < 3) {
        securityLevelDescription.value = "DÉBIL";
        securityWeak.value = true;
    } else if (securityLevel < 5) {
        securityLevelDescription.value = "MODERADO";
        securityMedium.value = true;
    } else if (securityLevel == 5) {
        securityLevelDescription.value = "ALTO";
        securityStrong.value = true;
    }
};

const formData = reactive({
    inputPasswordForm: "",
    inputRepeatPasswordForm: ""
});

const rules = {
    inputPasswordForm: {
        minLength: helpers.withMessage("La contraseña debe contener almenos 8 caracteres", minLength(8)),
        maxLength: helpers.withMessage("La contraseña no debe tener más de 20 caracteres", maxLength(20)),
        hasLowerCase: helpers.withMessage("La contraseña debe contener al menos un caracter minúsculo", hasLowerCase),
        hasNumbers: helpers.withMessage("La contraseña debe contener al menos un número", hasNumbers),
        hasUpperCase: helpers.withMessage("La contraseña debe contener al menos un caracter mayúsculo", hasUpperCase),
        hasSpecialCharacters: helpers.withMessage("La contraseña debe contener al menos un caracter especial (!%$&)", hasSpecialCharacters),
    },
    inputRepeatPasswordForm: {
        confirmPassword
    }

};

const v$ = useVuelidate(rules, formData);

onUpdated(() => {
    checkSecurityLevel(checkPassword(formData.inputPasswordForm));
});

const submit = async () => {
    const result = await v$.value.$validate();
    if (result) {
        alert("Contraseña correcta");
    } else {
        let errors = []
        for (let index = 0; index < v$.value.$errors.length; index++) {
            errors.push(v$.value.$errors[index].$message)
        }
    }
};
</script>