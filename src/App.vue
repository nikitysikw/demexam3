<script setup>
import {ref} from "vue";

const maps = [
    [
        [1, 0, 0],
        [0, 0, 0],
        [0, 0, 0]
    ],
    [
        [0, 0, 0],
        [0, 1, 0],
        [0, 0, 0]
    ],
    [
        [0, 0, 0],
        [0, 0, 0],
        [0, 0, 1]
    ]
]

let name = ref('')
let selectedMap = ref(false)
let map = ref([])

let status = ref(0)

function selectMap(numberMap) {
    selectedMap.value = true
    map.value = maps[numberMap]
}

let interval
let timer = ref(0)

function startGame() {
    status.value = 1

    clearInterval(interval)
    interval = setInterval(startTimer, 1000)
}

function startTimer() {
    timer.value = timer.value + 1

    if (timer.value === 10) {
        endGame()
    }
}

function endGame() {
    clearInterval(interval)
    status.value = 2
}

function resetGame() {
    timer.value = 0
    startGame()
}

</script>

<template>
    <div v-if="status === 0" class="container">
        <div class="d-flex justify-content-evenly pt-5">
            <div v-for="(map, number) in maps" :key="number">
                <div class="card align-items-center">
                    <div class="card-header">
                        <span>Карта: {{ number + 1 }}</span>
                    </div>
                    <div class="m-2">
                        <div v-for="(ma, number) in map" :key="number">
                            {{ ma }}
                        </div>
                    </div>
                </div>
                <button class="btn btn-outline-primary mt-2" @click="selectMap(number)">Выбрать</button>
            </div>
        </div>
        <div v-if="selectedMap" class="text-center mt-5">
            <div class="d-flex flex-column align-items-center">
                <input type="text" v-model="name" class="form-control w-50 text-center align-items-center" placeholder="Введите имя">
                <button type="button" class="btn btn-outline-primary mt-2 w-25" @click="startGame" :disabled="name === '' || name === ' '">Начать игру</button>
            </div>
        </div>
    </div>
    <div v-if="status === 1" class="d-flex flex-column align-items-center">
        <div v-for="(ma, number) in map" :key="number">
            <span v-for="(m, number) in ma" :key="number">
                <span>{{ m }}</span>
            </span>
        </div>
        <div class="d-flex flex-column">
            <span class="border rounded-2 p-2">Ваше имя: {{ name }}</span>
            <br>
            <span class="border rounded-2 p-2">Текущее время системы: {{ new Date().toLocaleTimeString() }}</span>
            <br>
            <span class="border rounded-2 p-2">Текущее время в игре: {{ (Math.floor(timer / 60 % 60) < 10) ? 0 : '' }}{{ Math.floor(timer / 60 % 60) }}:{{ timer % 60 < 10 ? 0 : '' }}{{ timer % 60 }}</span>
        </div>
    </div>
    <div v-if="status === 2" class="container d-flex justify-content-center align-content-center align-items-center flex-column">
        <span>Вы продержались: {{ Math.round(timer / 60 / 60) }} ч {{ Math.floor(timer / 60 % 60) }} мин {{ timer % 60 }} сек</span>
        <button type="button" class="btn-outline-primary btn" @click="resetGame">Перезапустить игру</button>
    </div>
</template>

<style scoped>

</style>
