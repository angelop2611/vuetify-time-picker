<template>
    <div class="d-flex">
        <div class="ma-2">
            <v-select density="compact" v-model="selectedHour" :error-messages="props.errors" :items="hours" :disabled="props.disabled" variant="outlined" hide-details></v-select>
        </div>
        <div class="ma-2">
            <span class="text-h4">:</span>
        </div>
        <div class="ma-2">
            <v-select density="compact" v-model="selectedMinute" :error-messages="props.errors" :items="minutes" :disabled="props.disabled" variant="outlined" hide-details></v-select>
        </div>
    </div>
</template>

<script setup>
import { ref, defineProps, watch, onMounted, defineEmits } from 'vue';

const props = defineProps({
    time: {
        type: String,
        default: '00:00'
    },
    errors: {
        type: String,
        default: null
    },
    disabled: {
        type: Boolean,
        default: false
    }
});

const hours = ref([
    '00', '01', '02', '03', '04', '05', '06', '07', '08', '09', '10', '11', '12',
    '13', '14', '15', '16', '17', '18', '19', '20', '21', '22', '23'
]);
const minutes = ref([
    '00', '10', '15', '20', '25', '30', '35', '40', '45', '50', '55'
]);

const selectedHour = ref('00');
const selectedMinute = ref('00');

const emit = defineEmits(['update:time'])

watch(() => props.time, newTime => {
    const [newHour, newMinute] = newTime.split(':');
    selectedHour.value = newHour;
    selectedMinute.value = newMinute;
});

watch([selectedHour, selectedMinute], () => {
    emit('update:time', selectedHour.value + ':' + selectedMinute.value);
});

onMounted(() => {
    const [newHour, newMinute] = props.time.split(':');
    selectedHour.value = newHour;
    selectedMinute.value = newMinute;
    emit('update:time', selectedHour.value + ':' + selectedMinute.value);
});
</script>
