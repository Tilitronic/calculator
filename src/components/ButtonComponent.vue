<script>
export default {
    props: ['type', 'label', 'onClick'],

}
</script>

<template>
        <q-btn 
            class="button"
            :outline="type==='return' ? false : true"
            :color="type==='operator'?'secondary': 'primary'"
            :label="label" 
            v-on:click="onClick(label)"
        />
</template>

<style scoped>

</style>
