<script setup lang="ts">
import { signOut, User } from '@firebase/auth'
import { useRouter } from 'vue-router'
import { auth } from '../../db'
import { useTodosStore } from '../../store/useTodosStore'

const store = useTodosStore()

const router = useRouter()

const user = auth.currentUser as User
const photoUrl = user.photoURL as string

const logout = async () => {
  await signOut(auth)
  store.clearTodos()
  router.push('/')
}
</script>

<template>
  <div class="pt-20">
    <div
      class="shadow-sm border-[0.5px] rounded-lg p-3 flex items-center justify-between px-6"
    >
      <div class="flex items-center gap-x-4">
        <img
          class="h-12 w-12 object-cover rounded-full"
          :src="
            photoUrl
              ? photoUrl
              : 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQmLbR6OHvoXjzY5UWbguFvw4HQcKZkfbKP7w&usqp=CAU'
          "
          alt="Current profile photo"
        />
        <div class="font-medium">{{ user.displayName }}</div>
      </div>

      <button
        @click="logout"
        type="submit"
        class="group disabled:bg-gray-400 relative flex font-bold justify-center text-purple-600 rounded-md border border-transparent bg-purple-100 py-2 px-4 text-sm hover:bg-purple-200 focus:outline-none focus:ring-0 focus:ring-offset-0"
      >
        Sign out
      </button>
    </div>
  </div>
</template>
