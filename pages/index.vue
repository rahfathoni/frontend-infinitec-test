<template>
  <div class="mx-auto max-w-screen-md px-6 pt-12">
    <div class="bg-gray-800 rounded-lg shadow">
      <div class="">
        <div
          v-for="o in limitData"
          :key="o.id"
          class="flex items-center px-6 py-4 list relative"
        >
          <div class="mr-5">
            <img v-if="o.avatar == ''"
              src="../assets/noimage.png"
              :alt="o.first_name"
              class="rounded-full"
              style="width: 50px"
            >
            <img v-else-if='o.avatar'
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
    <div class="py-2">
      <nav class="block">
        <ul class="flex pl-0 rounded list-none flex-wrap">
          <li v-for="n in total_pages"
            :key="n"
          >
            <a href="#" v-if="n === page"
              class="first:ml-0 text-xs font-semibold flex w-8 h-8 mx-1 p-0 rounded-full items-center justify-center leading-tight relative border border-solid border-indigo-500 text-white bg-indigo-500 hover:bg-opacity-25"
              @click.prevent="changePage(n)"
            >
              {{ n }}
            </a>
            <a href="#" v-else
              @click.prevent="changePage(n)"
              class="first:ml-0 text-xs font-semibold flex w-8 h-8 mx-1 p-0 rounded-full items-center justify-center leading-tight relative border border-solid border-pink-500 bg-white text-pink-500 hover:bg-opacity-25 hover:bg-indigo-400"
            >
              {{ n }}
            </a>
          </li>
        </ul>
      </nav>
    </div>
    <div class="flex pt-4 justify-center">
      <button class="button bg-green-600 uppercase text-white px-6 py-5 text-sm font-semibold shadow rounded hover:bg-green-400 focuse:outline-none"
        @click.prevent="toggleAddModal = true"
      >
        Add New user
      </button>
    </div>
    <UserAdd @addNewUser="addUser" :showAddModal="toggleAddModal" @close="toggleAddModal = false" />
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
      toggleDeleteModal: false,
      per_page: 4,
      page: 1,
      total_pages: 0
    }
  },
  computed: {
    limitData() {
      if(this.response.total > 1) {
        let prefix = (this.page-1) * this.per_page
        let suffix = this.per_page * this.page
        return this.response.data.slice(prefix, suffix)
      }
    },
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
      this.total_pages = Math.ceil(response.data.length / this.per_page)
    },
    addUser(newUser) {
      let array = this.response.data
      let idNewUser = array[array.length-1].id + 1
      this.data.push({
        id: idNewUser,
        ...newUser
      })
      this.total_pages = Math.ceil(this.data.length / this.per_page)
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
      this.total_pages = Math.ceil(this.data.length / this.per_page)
    },
    changePage(index) {
      this.page = index
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
