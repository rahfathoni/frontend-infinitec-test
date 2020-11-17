<template>
    <div v-if="showEditModal"
        class="fixed inset-0 w-full max-h-screen flex items-center justify-center bg-black bg-opacity-50 overflow-scroll"
    >
        <div class="w-full max-w-screen-sm bg-white rounded-lg p-8">
            <div class="text-2xl font-bold">EDIT USER DATA</div>
            <hr class="border-gray-600 border-solid">
            <form @submit.prevent="updateUser" class="m-5">
                <div class="md:flex md:item-center mb-3" for="first_name">
                    <div class="md:w-1/4">
                        <label class=" items-center uppercase block text-black font-bold md:text-right mb-1 md:mb-0 pr-4 pt-2">
                            First Name
                        </label>
                    </div>
                    <div class="md:w-3/4">
                        <input class="bg-gray-200 appearance-none border-2 border-gray-200 rounded-lg w-full px-4 py-2 text-black leading-tight focus:outline-none focus:bg-white focus:border-blue-200" 
                        type="text" placeholder="First Name..." v-model="editUser.first_name" id="first_name" name="first_name" required
                        >
                    </div>
                </div>
                <div class="md:flex md:item-center mb-3" for="last_name">
                    <div class="md:w-1/4">
                        <label class=" items-center uppercase block text-black font-bold md:text-right mb-1 md:mb-0 pr-4 pt-2">
                            Last Name
                        </label>
                    </div>
                    <div class="md:w-3/4">
                        <input class="bg-gray-200 appearance-none border-2 border-gray-200 rounded-lg w-full px-4 py-2 text-black leading-tight focus:outline-none focus:bg-white focus:border-blue-200" 
                        type="text" placeholder="Last Name..." v-model="editUser.last_name" id="last_name" name="last_name" required
                        >
                    </div>
                </div>
                <div class="md:flex md:item-center mb-3" for="email">
                    <div class="md:w-1/4">
                        <label class=" items-center uppercase block text-black font-bold md:text-right mb-1 md:mb-0 pr-4 pt-2">
                            Email
                        </label>
                    </div>
                    <div class="md:w-3/4">
                        <input class="bg-gray-200 appearance-none border-2 border-gray-200 rounded-lg w-full px-4 py-2 text-black leading-tight focus:outline-none focus:bg-white focus:border-blue-200" 
                        type="email" placeholder="Email..." v-model="editUser.email" id="email" name="email" required
                        >
                    </div>
                </div>
                <div class="md:flex md:item-center mb-3" for="avatar">
                    <div class="md:w-1/4">
                        <label class=" items-center uppercase block text-black font-bold md:text-right mb-1 md:mb-0 pr-4 pt-2">
                            Avatar
                        </label>
                    </div>
                    <div class="md:w-3/4">
                        <input class="bg-gray-200 appearance-none border-2 border-gray-200 rounded-lg w-full px-4 py-2 text-black leading-tight focus:outline-none focus:bg-white focus:border-blue-200" 
                        type="url" placeholder="Avater URL..." v-model="editUser.avatar" id="avatar" name="avatar"
                        >
                    </div>
                </div>
                <div class="flex justify-center mb-3">
                    <img v-if="editUser.avatar === ''"
                        src="../assets/noimage.png"
                        alt="input picture"
                        class="rounded-full w-32"
                    >
                    <img v-else-if='editUser.avatar'
                        :src="editUser.avatar"
                        alt="url not working"
                        class="rounded-full w-32"
                    >
                </div>
                <div class="flex items-center justify-around">
                    <button class="button bg-blue-600 text-white px-8 py-3 mt-3 text-sm font-semibold rounded hover:bg-blue-400 focuse:outline-none"
                        type="submit"
                    >
                        ADD
                    </button>
                    <button class="button bg-red-500 text-white px-8 py-3 mt-3 text-sm font-semibold rounded hover:bg-red-300 focuse:outline-none"
                        @click.prevent="close"
                    >
                        CANCEL
                    </button>
                </div>
            </form>
        </div>
    </div>
</template>

<script>
export default {
    props: ['showEditModal'],
    data() {
        return {
            editUser: {
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
        inputEditData(data) {
            this.editUser.id = data.id
            this.editUser.email = data.email
            this.editUser.first_name = data.first_name
            this.editUser.last_name = data.last_name
            this.editUser.avatar = data.avatar
        },
        updateUser() {
            this.$emit('updateUser', this.editUser)
            this.$emit('close')
        },
    }
}
</script>

<style>

</style>