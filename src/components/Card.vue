<!-- Card Component -->
<script setup>

// Definir propiedades del componente
// La función recibe un parámetro que es un objeto con todas las propiedades del componente
// Se pone nombredelapropiedad: tipo de variable

const props = defineProps({
    back: String, // es una ruta a una imagen
    front: String, // es una ruta a una imagen
    reveal: Boolean // me dice que debo revelarme o no
})

</script>

<template>
    <!-- Podemos usar en el template directamente las propiedades, simplemente poniendo su nomre -->

    <img v-if="!reveal" :src="back" alt="pokemon card">
    <img v-else :src="front" alt="pokemon butterfly">

</template>

<style scoped>
img {
    width: clamp(69px, 18vw, 175px);
}
</style>