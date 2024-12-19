<template>
    <div class="row">
        <div class="content_select">
            <label for="">Available Options</label>
            <select @click="handleDisabled" multiple>
                <option v-for="option in available" :value="option.id">{{ option.label }}</option>
            </select>
        </div>

        <div class="content_select">
            <label for="">Desabled Options</label>
            <select @click="handleAvailable" multiple>
                <option v-for="option in desabled" :value="option.id">{{ option.label }}</option>
            </select>
        </div>
    </div>

</template>

<script setup>
import { ref, watch } from "vue";
import fieldMixin from '../FieldMixin';
const emit = defineEmits(['update']);
const props = defineProps({
    field: {
        type: Object,
        required: true,
    },
    modelValue: {
        type: [String, Number, Boolean, Object, Array],
        default: '',
    },
});

const available = ref(props.field.options)
const desabled = ref([])
const valueDesabled = ref([]);

let { valueFromEvent } = fieldMixin.setup(props, { emit });

const handleDisabled = (event) => {
    desabled.value.push(available.value.find(opt => opt.id == event.target.value))
    available.value = available.value.filter(opt => opt.id != event.target.value)

    valueDesabled.value.push(event.target.value);
    emit("update", { id: props.field.id, value: valueDesabled.value })
};

const handleAvailable = (event) => {
    available.value.push(desabled.value.find(opt => opt.id == event.target.value))
    desabled.value = desabled.value.filter(opt => opt.id != event.target.value)

    valueDesabled.value = valueDesabled.value.filter(v => v != event.target.value)
    emit("update", { id: props.field.id, value: valueDesabled.value })
};

</script>
<style lang="css" scoped>

.row{
    display: flex;
    width: 100%;
    gap: 20px;
    margin-bottom: 16px;
}

.content_select {
    width: 100%;
}

select {
    overflow-y: auto;
    width: 100%;
    height: 120px;
}
</style>