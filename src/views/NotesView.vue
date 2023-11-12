<script setup>
import { mdiBallotOutline} from '@mdi/js'
import SectionMain from '@/components/SectionMain.vue'
import CardBox from '@/components/CardBox.vue'
import BaseButton from '@/components/BaseButton.vue'
import BaseButtons from '@/components/BaseButtons.vue'
import LayoutAuthenticated from '@/layouts/LayoutAuthenticated.vue'
import SectionTitleLineWithButton from '@/components/SectionTitleLineWithButton.vue'
import Editor  from '@tinymce/tinymce-vue';
</script>

<template>
  <LayoutAuthenticated>
    <SectionMain>
      <SectionTitleLineWithButton :icon="mdiBallotOutline" title="Notes" main>
      </SectionTitleLineWithButton>
      <CardBox form @submit.prevent="submit">
      <div>
        <Editor api-key="your-api-key" :init="{ menubar: false , height: 600, }"></Editor>
      </div>
        <template #footer>
          <BaseButtons>
            <BaseButton type="submit" color="info" label="Submit" />
            <BaseButton type="reset" color="info" outline label="Reset" />
          </BaseButtons>
        </template>
      </CardBox>
    </SectionMain>
  </LayoutAuthenticated>
</template>
