<template>
  <div class="flex flex-col items-center bg-white rounded-xl p-4 w-fit">
    <div class="flex items-center m-6 space-x-4">
      <img class="h-14" src="../assets/robot_saludo.de0acfb2.png" alt="" />
      <h1 class="font-semibold text-4xl">Restablezca su contraseña</h1>
    </div>
    <div class="flex space-x-3">
      <input
        v-model="inputPassword"
        class="w-80 p-2 border-2 rounded-md border-green-400"
        :type="passwordVisible"
        name=""
        id=""
        placeholder="contraseña"
        maxlength="20"
      />
      <img
        class="cursor-pointer"
        @click="showPassword"
        :src="eyeIcon"
        alt="Mostrar contraseña"
      />
    </div>

    <div class="flex mt-2 items-center">
      <p class="text-lg">Nivel de seguridad:</p>
      <p :class="{ 'fill-green-500': securityWeak }" class="text-lg">
        {{ securityLevelDescription }}
      </p>
      <svg
        :class="{'fill-red-500': securityWeak, 'fill-yellow-500': securityMedium, 'fill-green-500': securityStrong}"
        height="24"
        viewBox="0 0 24 24"
        width="24"
        xmlns="http://www.w3.org/2000/svg"
      >
        <path
          d="m21 11c0 5.55-3.84 10.74-9 12-5.16-1.26-9-6.45-9-12v-6l9-4 9 4zm-9 10c3.75-1 7-5.46 7-9.78v-4.92l-7-3.12-7 3.12v4.92c0 4.32 3.25 8.78 7 9.78m2.8-10v-1.5c0-1.4-1.4-2.5-2.8-2.5s-2.8 1.1-2.8 2.5v1.5c-.6 0-1.2.6-1.2 1.2v3.5c0 .7.6 1.3 1.2 1.3h5.5c.7 0 1.3-.6 1.3-1.2v-3.5c0-.7-.6-1.3-1.2-1.3m-1.3 0h-3v-1.5c0-.8.7-1.3 1.5-1.3s1.5.5 1.5 1.3z"
        />
      </svg>
    </div>
    <p class="mt-4 text-xl">La contraseña debe contener:</p>
    <div class="flex flex-col items-start text-xl">
      <div class="flex justify-center items-center">
        <img
          :class="{ hidden: !checkEightCharacters }"
          class="h-4"
          src="../assets/heavy-check-mark.svg"
          alt=""
        />
        <p>8 caracteres como mínimo</p>
      </div>
      <div class="flex justify-center items-center">
        <img
          :class="{ hidden: !checkLowerCase }"
          class="h-4"
          src="../assets/heavy-check-mark.svg"
          alt=""
        />
        <p>Minúsculas</p>
      </div>
      <div class="flex justify-center items-center">
        <img
          :class="{ hidden: !checkUpperCase }"
          class="h-4"
          src="../assets/heavy-check-mark.svg"
          alt=""
        />
        <p>Mayúsculas</p>
      </div>
      <div class="flex justify-center items-center">
        <img
          :class="{ hidden: !checkSpecialCharacters }"
          class="h-4"
          src="../assets/heavy-check-mark.svg"
          alt=""
        />
        <p>Símbolos (!$#%)</p>
      </div>
      <div class="flex justify-center items-center">
        <img
          :class="{ hidden: !checkNumbers }"
          class="h-4"
          src="../assets/heavy-check-mark.svg"
          alt=""
        />
        <p>Números</p>
      </div>
    </div>
    <div class="flex items-start">
      <p class="text-red-600">*hola</p>
    </div>
    <button
      @click="submit"
      class="flex mb-6 w-fit p-5 bg-blue-900 rounded-full text-white"
    >
      <img class="invert mr-3" src="../assets/arrow-right-1.svg" alt="" />
      Restablecer contraseña
    </button>
  </div>
</template>

<script setup>
import { ref, reactive, onUpdated } from "vue";
import { useVuelidate } from "@vuelidate/core";
import { required, minLength, maxLength } from "@vuelidate/validators";

const inputPassword = ref("");
const passwordVisible = ref("password");
const eyeIcon = ref("/src/assets/eye-password-hide.svg");

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

const checkPassword = (str) => {
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

const securityWeak = ref(false);
const securityMedium = ref(false);
const securityStrong = ref(false);
const securityLevelDescription = ref("NULO");

const checkSecurityLevel = (securityLevel) => {
    securityWeak.value = false
    securityMedium.value = false
    securityStrong.value = false
  if (securityLevel < 3) {
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
  inputPasswordForm: inputPassword.value,
});

const rules = {
  inputPasswordForm: {
    required,
    minLength: minLength(8),
    maxLength: maxLength(20),
    hasLowerCase,
    hasNumbers,
    hasUpperCase,
    hasSpecialCharacters,
  },
};

const v$ = useVuelidate(rules, inputPassword.value);

console.log(hasUpperCase("AAAA"));

onUpdated(() => {
  checkSecurityLevel(checkPassword(inputPassword.value));
});

const submit = async () => {
  console.log(formData.inputPasswordForm);
  const result = await v$.value.$validate();
  console.log(result);
  if (result) {
    alert("Contraseña correcta");
  } else {
    alert("Contraseña incorrecta");
  }
};
</script>
