<template>
    <div v-if="showDeleteModal"
        class="fixed inset-0 w-full max-h-screen flex items-center justify-center bg-black bg-opacity-50 overflow-scroll"
    >
        <div class="w-full max-w-screen-sm bg-white rounded-lg p-8">
            <div class="text-2xl font-bold">DELETE THIS USER ?</div>
            <hr class="border-gray-600 border-solid">
            <div class="justify-center flex mt-2">
                <img v-if="deleteUser.avatar === ''"
                    src="../assets/noimage.png"
                    alt="input picture"
                    class="rounded-full w-32"
                >
                <img v-else-if='deleteUser.avatar'
                    :src="deleteUser.avatar"
                    alt="url not working"
                    class="rounded-full w-32"
                >
            </div>
            <div class=" text-3xl font-bold text-center mt-1">
                {{deleteUser.first_name}} {{deleteUser.last_name}}
            </div>
            <div class=" text-3xl text-center mt-0">
                ({{deleteUser.email}})
            </div>
            <div class="flex items-center justify-around mt-2">
                <button class="button bg-red-600 text-white px-8 py-3 mt-3 text-sm font-semibold rounded hover:bg-red-400 focuse:outline-none"
                    @click.prevent="removeUser"
                >
                    DELETE
                </button>
                <button class="button bg-yellow-500 text-white px-8 py-3 mt-3 text-sm font-semibold rounded hover:bg-yellow-300 focuse:outline-none"
                    @click.prevent="close"
                >
                    CANCEL
                </button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: ['showDeleteModal'],
    data() {
        return {
            deleteUser: {
                id: '',
                email: '',
                first_name: '',
                last_name: '',
                avatar: '',
            }
        }
    },
    methods: {
        close() {
            this.$emit('close')
        },
        inputDeleteData(data) {
            this.deleteUser.id = data.id
            this.deleteUser.email = data.email
            this.deleteUser.first_name = data.first_name
            this.deleteUser.last_name = data.last_name
            this.deleteUser.avatar = data.avatar
        },
        removeUser() {
            this.$emit('removeUser', this.deleteUser)
            this.$emit('close')
        },
    }   
}
</script>

<style>

</style>