<script setup>
import { ref } from 'vue';
import SectionFullScreen from '@/components/SectionFullScreen.vue'
import LayoutAuthenticated from '@/layouts/LayoutAuthenticated.vue';
import BaseIcon from '@/components/BaseIcon.vue';
import { mdiClose, mdiCheck } from '@mdi/js';

const questions = ref([
    {
        "question": "Is the Earth round?",
        "status": true
    },
    {
        "question": "Does the sun revolve around the Earth?",
        "status": false
    },
    {
        "question": "Is water composed of two hydrogen atoms and one oxygen atom?",
        "status": true
    },
    {
        "question": "Can humans breathe naturally in space without any special equipment?",
        "status": false
    },
    {
        "question": "Is the boiling point of water 100 degrees Celsius at sea level?",
        "status": true
    }
    // ... other questions
]);

const currentIndex = ref(0);
const correctCount = ref(0);
const wrongCount = ref(0);
const start = ref(true)
const translateX = ref(0);

const handleSwipe = (isCorrect) => {
    translateX.value += isCorrect ? 50 : -50;
    setTimeout(() => {
        translateX.value -= isCorrect ? 50 : -50;
        isCorrect === questions.value[currentIndex.value].status ? correctCount.value++ : wrongCount.value++;
        if (currentIndex.value < questions.value.length-1) {
            currentIndex.value++;
        }
        else if(currentIndex.value == questions.value.length-1){
           
            start.value = false
            currentIndex.value=0
            return;
        }
    }, 100);
};


const handleSwipeLeft = () => handleSwipe(false);
const handleSwipeRight = () => handleSwipe(true);

const restart = ()=>{
    currentIndex.value=0
    correctCount.value=0
    wrongCount.value=0
    start.value = true
}

</script>

   
<template>
    <LayoutAuthenticated>
        <SectionFullScreen>
            <div v-if="start" class="w-72 sm:w-96 mb-16">
                <div class="text-center mb-2 flex flex-center justify-center gap-1">
                   <p class="text-red-600">{{ wrongCount }}</p> / <p class="text-green-600">{{  correctCount }}</p>
                </div>
                <div class=" bg-gradient-to-r from-red-600 to-green-600 rounded-3xl h-80">
                    <div
v-touch:swipe.left="handleSwipeLeft"
                        v-touch:swipe.right="handleSwipeRight" class="card bg-white dark:bg-slate-900 p-8 rounded-3xl shadow-lg mx-4 h-80 text-xl"
                        :style="{ transform: `translateX(${translateX}px)` }">
                        {{ questions[currentIndex].question }}
                    </div>
                </div>
               
                <div class="flex pt-4 items-center justify-center gap-2">
                    <button class="rounded-full hover:shadow-lg py-2" @click.prevent="handleSwipeLeft">
                        <BaseIcon :path="mdiClose" class="flex-none" w="w-12" :size="24" />
                    </button>
                    <div class="w-20 text-center">
                        {{ currentIndex + 1 }} / {{ questions.length }}
                    </div>
                    <button class="rounded-full hover:shadow-lg py-2" @click.prevent="handleSwipeRight">
                        <BaseIcon :path="mdiCheck" class="flex-none" w="w-12" :size="24" />
                    </button>
                </div>
            </div>
            <div v-else class="flex flex-col ">
                {{"Game Over! You got "+correctCount+" out of "+questions.length +" Correct"}}
                <button class="rounded-xl mx-auto hover:shadow-lg p-4 font-bold mt-4 underline" @click.prevent="restart">
                        start over
                    </button>
            </div>
        </SectionFullScreen>
    </LayoutAuthenticated>
</template>

<style scoped>
.card {
    transition: transform 0.1s ease-out;
}
</style>