<template>
    <div class="flex items-center space-x-3">
        <input class="w-80 h-10 p-2 border-2 rounded-md border-green-400"
            :type="passwordVisible" name="" id="" placeholder="Repetir contraseña" maxlength="20" />
        <img class="cursor-pointer h-8" @click="showPassword" :src="eyeIcon" alt="Mostrar contraseña" />
    </div>
</template>

<script setup>
import { ref, reactive, onUpdated } from "vue";
const passwordVisible = ref("password");
const eyeIcon = ref("/src/assets/eye-password-hide.svg");
const showPassword = () => {
    if (passwordVisible.value === "password") {
        passwordVisible.value = "text";
        eyeIcon.value = "/src/assets/eye-password-show.svg";
    } else {
        passwordVisible.value = "password";
        eyeIcon.value = "/src/assets/eye-password-hide.svg";
    }
};
</script>