<script setup>
import SectionMain from '@/components/SectionMain.vue'
import CardBox from '@/components/CardBox.vue'
import LayoutAuthenticated from '@/layouts/LayoutAuthenticated.vue'
import { DatePicker } from 'v-calendar';
import 'v-calendar/style.css';
import { ref,reactive } from 'vue'

import { registerLicense } from '@syncfusion/ej2-base';
import { provide } from "vue";
import {
    ScheduleComponent as EjsSchedule,
    ResourcesDirective as EResources, ResourceDirective as EResource,
    Day
} from "@syncfusion/ej2-vue-schedule";

registerLicense('Ngo9BigBOggjHTQxAR8/V1NHaF5cXmVCf1JpRGtGfV5yd0VHYlZTRHxeRk0DNHVRdkdgWH5fcXVXRGBdVUF1WEY=');
provide('schedule', [Day]);

const eventSettings = reactive({
    dataSource: [
        {
            Id: 1,
            Subject: 'Surgery - Andrew',
            EventType: 'Confirmed',
            StartTime: new Date(Date.now() + Math.random() * 1000 * 60 * 60 * 24 * 30), // Random date from now
           EndTime: new Date(Date.now() + Math.random() * 1000 * 60 * 60 * 24 * 30), // Random date from now
            OwnerId: 2
        },
        {
            Id: 2,
            Subject: 'Consulting - John',
            EventType: 'Confirmed',
            StartTime: new Date(Date.now() + Math.random() * 1000 * 60 * 60 * 24 * 30), // Random date from now
           EndTime: new Date(Date.now() + Math.random() * 1000 * 60 * 60 * 24 * 30), // Random date from now
            OwnerId: 3,
        },
        {
            Id: 3,
            Subject: 'Therapy - Robert',
            EventType: 'Requested',
            StartTime: new Date(Date.now() + Math.random() * 1000 * 60 * 60 * 24 * 30), // Random date from now
           EndTime: new Date(Date.now() + Math.random() * 1000 * 60 * 60 * 24 * 30), // Random date from now
            OwnerId: 1
        }
    ]
});
const ownerDataSource = reactive([
    { OwnerText: "Nancy", Id: 1, OwnerColor: "#ffaa00" },
    { OwnerText: "Steven", Id: 2, OwnerColor: "#f8a398" },
    { OwnerText: "Michael", Id: 3, OwnerColor: "#7499e1" }
]);
const selectedColor = ref('blue');

const date = ref(new Date())
</script>

<template>
    <LayoutAuthenticated>
        <SectionMain >
            <div class="p-8">
            </div>
            <DatePicker v-model="date" expanded view="weekly" :color="selectedColor"/>
            <div class="p-4">
            </div>
            <CardBox class="mb-6" has-table>

            <ejs-schedule height='550px' width='100%' :selected-date='date' :event-settings='eventSettings' :show-header-bar="false">
                <e-resources>
                    <e-resource
field="OwnerId" title="Owner" name="Owners" :data-source="ownerDataSource"
                        text-field="OwnerText" id-field="Id" color-field="OwnerColor">
                    </e-resource>
                </e-resources>
            </ejs-schedule>
            </CardBox>


        </SectionMain>
    </LayoutAuthenticated>
</template>
