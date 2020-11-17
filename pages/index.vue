<template>
  <div class="mx-auto max-w-screen-md px-6 pt-12">
    <div class="bg-gray-800 rounded-lg shadow">
      <div class="">
        <div
          v-for="o in data"
          :key="o.id"
          class="flex items-center px-6 py-4 list relative"
        >
          <div class="mr-5">
            <img
              :src="o.avatar"
              :alt="o.first_name"
              class="rounded-full"
              style="width: 50px"
            >
          </div>
          <div>
            <nuxt-link to="/" class="font-semibold text-white">
              {{ o.email }}
            </nuxt-link>
            <div class="text-gray-500">
              {{ o.first_name }} {{ o.last_name }}
            </div>
          </div>
          <div class="ml-auto">
            <a
              href="#"
              @click.prevent="inputEditModal(o)"
              class="uppercase text-sm text-white text-opacity-75 hover:text-opacity-100 font-semibold"
            >
              Edit
            </a>
            <a
              href="#"
              @click.prevent="inputDeleteModal(o)"
              class="uppercase text-sm text-red-500 text-opacity-75 hover:text-opacity-100 font-semibold"
            >
              Delete
            </a>
          </div>
        </div>
      </div>
    </div>
    <div class="flex pt-4 justify-center">
      <button class="button bg-green-600 uppercase text-white px-6 py-5 text-sm font-semibold shadow rounded hover:bg-green-400 focuse:outline-none"
        @click.prevent="toggleAddModal = true"
      >
        Add New user
      </button>
    </div>
    {{this.data}}
    <UserAdd @addNewUser="addUser" :showAddModal="toggleAddModal" @close="toggleAddModal = false"/>
    <UserEdit @updateUser="updateUser" ref="editUserData" :showEditModal="toggleEditModal" @close="toggleEditModal = false" />
    <UserDelete @removeUser="removeUser" ref="deleteUserData" :showDeleteModal="toggleDeleteModal" @close="toggleDeleteModal = false" />
  </div>
</template>

<script>
import UserAdd from "../components/UserAdd";
import UserEdit from "../components/UserEdit";
import UserDelete from "../components/UserDelete";
export default {
  components: {
    UserAdd, UserEdit, UserDelete
  },
  data() {
    return {
      response: {},
      toggleAddModal: false,
      toggleEditModal: false,
      toggleDeleteModal: false
    }
  },
  computed: {
    data() {
      return this.response.data
    },
    meta() {
      const {
        page,
        per_page,
        total_pages,
        total
      } = this.response

      return {
        page,
        per_page,
        total_pages,
        total,
      }
    }
  },
  mounted() {
      this.getData()
  },
  methods: {
    async getData() {
      const response = await this.$axios.$get('users')
      this.response = response
    },
    addUser(newUser) {
      let array = this.response.data
      let idNewUser = array[array.length-1].id + 1
      this.data.push({
        id: idNewUser,
        ...newUser
      })
    },
    inputEditModal(userData) {
      this.toggleEditModal = true;
      this.$refs.editUserData.inputEditData(userData)
    },
    updateUser(newData){
      for(let i = 0; i < this.data.length; i++){
        if(this.data[i].id == newData.id){
          this.data[i].email = newData.email
          this.data[i].first_name = newData.first_name
          this.data[i].last_name = newData.last_name
          this.data[i].avatar = newData.avatar
          break
        }
      }
    },
    inputDeleteModal(userData){
      this.toggleDeleteModal = true;
      this.$refs.deleteUserData.inputDeleteData(userData)
    },
    removeUser(userData){
      for(let i = 0; i < this.data.length; i++){
        if(this.data[i].id == userData.id){
          this.data.splice(i, 1)
          break
        }
      }
    }
  }
}
</script>

<style>
.list:after {
  content: '';
  @apply border-b absolute bottom-0 left-0 right-0 ml-24 border-gray-700;
}
</style>
