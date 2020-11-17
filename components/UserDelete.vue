<template>
    <div v-if="showDeleteModal"
        class="fixed inset-0 w-full h-screen flex items-center justify-center"
    >
        <div class="w-full max-w-2xl bg-white shadow-lg rounded-lg p-8">
            <div>
                modal Delete here    
            </div>
            <button class="button bg-red-600 text-white px-6 py-5 text-sm font-semibold shadow rounded hover:bg-red-400 focuse:outline-none"
                @click.prevent="close"
            >
            close
      </button>
        </div>
    </div>
</template>

<script>
export default {
    props: ['showDeleteModal'],
    methods: {
        close() {
            this.$emit('close')
        }
    }   
}
</script>

<style>

</style>