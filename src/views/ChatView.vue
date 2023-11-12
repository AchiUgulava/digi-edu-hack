<script setup>
// import { reactive } from "vue";
import { useMainStore } from "@/stores/main";
import { ref, onMounted } from "vue";
import { useRoute } from "vue-router";
import { mdiBallotOutline } from "@mdi/js";
import SectionMain from "@/components/SectionMain.vue";
import FormField from "@/components/FormField.vue";
import FormControl from "@/components/FormControl.vue";
// import BaseDivider from "@/components/BaseDivider.vue";
import BaseButton from "@/components/BaseButton.vue";
import LayoutAuthenticated from "@/layouts/LayoutAuthenticated.vue";
import SectionTitleLineWithButton from "@/components/SectionTitleLineWithButton.vue";
import router from "@/router";

const mainStore = useMainStore()


const route = useRoute();
const id = ref("");

onMounted(() => {
  id.value = route.params.id;
});

const questions = ref([
  {
    id: 234,
    type: "question",
    content: `Hello ${mainStore.userName}, How can i help you`
  },
  {
    id: 1234,
    type: "question",
    content: "Please Try again Later"
  },
  {
    id: 1234,
    type: "question",
    content: "Please  try again later"
  }
]
);
const messages = ref([]);
const currentQuestionIndex = ref(0);
const answer = ref("");
onMounted(() => {
 
  if (questions.value.length > 0) {
    messages.value.push(questions.value[0]);
  }
});

const submitAnswer = () => {
  if (answer.value) {
    messages.value.push({
      id: currentQuestionIndex.value,
      type: "answer",
      content: answer.value,
    });
    if (currentQuestionIndex.value < questions.value.length -1) {
      currentQuestionIndex.value++;
      messages.value.push(questions.value[currentQuestionIndex.value]);
    }
    answer.value = "";
  }
  if(currentQuestionIndex.value == 2){
    setTimeout(()=>{
        router.push('/calendar')
    },3000)
  }
};

</script>

<template>
  <LayoutAuthenticated>
    <SectionMain>
      <SectionTitleLineWithButton :icon="mdiBallotOutline" title="Chat" main>
      </SectionTitleLineWithButton>
      <div
        class="container mx-auto h-[90vh] space-y-4 flex-grow overflow-y-auto pb-4"
      >
        <div
          v-for="message in messages"
          :key="message.id"
          :class="{
            'bg-gray-700 text-white': message.type === 'question',
            'bg-blue-600 text-white': message.type === 'answer',
          }"
          class="rounded-lg py-4 px-6 sm:text-lg"
        >
          {{ message.content }}
        </div>
      </div>
      <div class="fixed bottom-0 left-0 w-full bg-white dark:bg-slate-900 xl:pl-60">
        <SectionMain>

        <div class="form-field mt-4">
          <FormField >
            <FormControl
              v-model="answer"
              type="textarea"
              placeholder="Explain how we can help you"
            />
          </FormField>
          </div >
          <div class="w-full flex felx-end justify-end">

          <div class="absolute bottom-8">
              <BaseButton
                type="submit"
                color="info"
                label="Submit"
                @click="submitAnswer"
              />
          </div>
        </div>
    </SectionMain>

    </div>
    </SectionMain>
  </LayoutAuthenticated>
</template>