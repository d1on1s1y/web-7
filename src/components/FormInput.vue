<template>
    <div class="form-input">
        <label>{{label}}</label>
        <input :type="type" :value="modelValue" @input="onInput" class="input-field" :placeholder="placeholder" />
        <p v-if="error" class="error">{{ error }}</p>
    </div>
</template>

<script>


export default {
    props: {
        label: String,
        modelValue: String,
        error: String,
        placeholder: String,
        type: {
            type: String,
            default: 'text'
        }
    },methods:{
        onInput(event) {
            this.$emit('update:modelValue', event.target.value);
            this.$emit('validate');
        }
    }
}

</script>

<style></style>