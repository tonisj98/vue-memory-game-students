<!-- ChessBoard Component -->
<script setup>

import { reactive } from 'vue';
import Card from './Card.vue';

const props = defineProps({
    cards: Array,
    backCardImage: String
})

// Vamos a definir un evento que se va a disparar cuando dos parejas seleccionadas sean correctas
const emit = defineEmits(['onCheckedCards'])

// Añadimos una nueva propiedad al array de cartas
// reactive va a hacer que a partir de ahora este componente sea reactivo a los cambios en cualquiera de sus elementos

// dobla el numero de cartas del array
const cardsDoubled = props.cards.concat(props.cards); // TODO: Dobla el número de cartas. Sugerencia: sale bastante fácil usando el operador de 'spread' (...) y el método .concat.

// Variable de estado que guarde las cartas seleccionadas hasta el momento
const selectedCards = [];



console.log(cardsDoubled);

const cardsChessBoard = reactive(cardsDoubled.map((card) => {
    return {
        ...card,
        reveal: false
    }
}));

shuffleArray(cardsChessBoard);

function shuffleArray(inputArray) {
    inputArray.sort(() => Math.random() - 0.5);
}

function onCardClicked(card) {

    // Evitar que podamos hacer click en una carta ya revealda
    if (card.reveal || selectedCards.length == 2) {
        return;
    }

    // Primero, siempre mostramos la carta seleccionada. Reactivamente, el estado cambia, y entonces se actualiza el DOM del template 
    card.reveal = true;

    // Añadimos la carta seleccionada al array de cartas seleccionadas
    selectedCards.push(card);

    // Si ya hemos seleccionado 2 cartas, hay que compararlas
    if (selectedCards.length == 2) {
        // hemos hecho match?

        const isMatch = selectedCards[0].id == selectedCards[1].id;

        // emitimos un evento al padre diciendo si hemos hecho match (true o false)
        emit('onCheckedCards', isMatch)

        // Si su id NO es el mismo, hemos errado. Tenemos que darle la vuelta a la cartas.
        if (selectedCards[0].id != selectedCards[1].id) {
            // Con el this podemos acceder a toda la instancia del script
            setTimeout(() => {
                selectedCards[0].reveal = false;
                selectedCards[1].reveal = false;
                selectedCards.pop();
                selectedCards.pop();
            }, 1000)

        }
        else {
            // la pareja es correcta. Hay que emitir un evento indicando que las dos cartas seleccionadas son iguales
            selectedCards.pop();
            selectedCards.pop();

        }
        // En cualquier caso, tenemos que 'reiniciar' el estado de la app para que podamos seleccionar las dos siguientes cartas
    }


}


</script>

<template>
    <!-- Iteración 3: Utiliza adecuadamente el v-for para crear tantas Card como elementos hay en 'cards'. -->
    <div>
        <Card @click="onCardClicked(card)" v-for="card in cardsChessBoard" :key="card.id" :back="backCardImage"
            :front="card.image" :reveal="card.reveal">
        </Card>
    </div>

</template>

<style scoped>
div {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr 1fr 1fr;
    row-gap: 1rem;
    cursor: pointer;
    user-select: none;

}
</style>