<script setup>
import { ref } from "vue";

const isModal = ref(false);
const showModal = () => {
    isModal.value = !isModal.value;
};

function getRandomColor() {
    const color = "hsl(" + Math.random() * 360 + ", 100%, 75%)";
    return color;
}

const errorMessage = ref(``);
const newNote = ref(``);
const notes = ref([]);
const addNote = () => {
    if (newNote.value.length < 10) {
        return (errorMessage.value = `Note must be at least 10 characters long.`);
    }

    notes.value.push({
        id: Math.floor(Math.random() * 100000),
        text: newNote.value,
        // date: new Date().toString().substring(0, 15).split(` `).join(`/`),
        date: new Date().toLocaleDateString(),
        backgroundColor: getRandomColor(),
    });
    isModal.value = !isModal.value;
    newNote.value = ``;
    errorMessage.value = ``;
};
</script>

<template>
    <main>
        <div class="overlay" v-if="isModal">
            <div class="modal">
                <textarea
                    v-model.trim="newNote"
                    name="note"
                    id="note"
                    cols="30"
                    rows="10"
                ></textarea>
                <p v-if="errorMessage">{{ errorMessage }}</p>
                <button @click="addNote">Add note</button>
                <button class="close" @click="showModal">Close</button>
            </div>
        </div>
        <div class="container">
            <header>
                <h1>Notes</h1>
                <button @click="showModal">+</button>
            </header>
            <div class="cards-container">
                <div
                    v-for="note in notes"
                    :key="note.id"
                    :style="{ backgroundColor: note.backgroundColor }"
                    class="card"
                >
                    <p class="main-text">{{ note.text }}</p>
                    <p class="date">{{ note.date }}</p>
                </div>
            </div>
        </div>
    </main>
</template>
<style scoped>
main {
    min-height: 100vh;
    position: relative;
}

.container {
    max-width: 1000px;
    padding: 10px;
    margin: 0 auto;
}

header {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

h1 {
    font-weight: bold;
    margin-bottom: 25px;
    font-size: 75px;
}

header button {
    border: none;
    padding: 10px;
    width: 50px;
    height: 50px;
    cursor: pointer;
    background-color: var(--vt-c-white-mute);
    border-radius: 100%;
    font-size: 20px;
}

.card {
    width: 225px;
    height: 225px;
    background-color: rgb(237, 182, 44);
    padding: 10px;
    border-radius: 15px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    margin-right: 20px;
    margin-bottom: 20px;
    color: black;
}

.date {
    font-size: 10px;
    font-weight: bold;
}

.cards-container {
    display: flex;
    flex-wrap: wrap;
}

.overlay {
    position: absolute;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.77);
    z-index: 10;
    display: flex;
    justify-content: center;
    align-items: center;
}

.modal {
    width: 750px;
    background-color: white;
    border-radius: 10px;
    padding: 30px;
    position: relative;
    display: flex;
    flex-direction: column;
}
.modal button {
    padding: 10px 20px;
    font-size: 20px;
    width: 100%;
    background-color: blueviolet;
    border: none;
    color: white;
    cursor: pointer;
    margin-top: 15px;
}

.modal .close {
    background-color: rgb(147, 13, 13);
    margin-top: 10px;
}
.modal p {
    font-size: 15px;
    color: crimson;
    cursor: pointer;
}
textarea {
    width: 100%;
    height: 200px;
    padding: 5px;
    font-size: 20px;
}
</style>
