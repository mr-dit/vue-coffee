<script setup>
import { useAuthStore } from '@/store'
import axios from 'axios'
import { telegramLoginTemp } from 'vue3-telegram-login'

defineProps({
  totalPrice: Number
})
const emit = defineEmits(['openDrawer'])

const store = useAuthStore()

const onLogin = async (user) => {
  // TODO надо куда-то в стор
  try {
    const res = await axios.post(`${import.meta.env.VITE_HOST_API}auth/telegram`, user, {
      withCredentials: true // Обязательно для отправки куки
    })
    console.log('-----', res)
    store.setUser(res.data)
  } catch (e) {
    console.log(e)
  }
}
</script>

<template>
  <header class="flex justify-between border-b border-slate-300 px-8">
    <RouterLink to="/">
      <div class="flex items-center gap-4">
        <img src="/logo.png" alt="Logo" class="w-10" />
        <div>
          <h2 class="text-xl font-bold uppercase">МышАтте</h2>
          <p class="text-gray-500">Всегда самый вкусный</p>
        </div>
      </div>
    </RouterLink>

    <!-- TODO скрывать кнопки до входа в акк и придумать кнопку выхода -->
    <ul class="flex items-center gap-10">
      <div class="flex items-center gap-10" v-if="store.isAuth">
        <li
          @click="emit('openDrawer')"
          class="flex items-center cursor-pointer gap-3 text-gray-500 hover:text-black"
        >
          <img src="/cart.svg" alt="Cart" />
          <b>{{ totalPrice }} руб.</b>
        </li>

        <router-link to="/favorites">
          <li class="flex items-center cursor-pointer gap-3 text-gray-500 hover:text-black">
            <img src="/heart.svg" alt="Heart" />
            <span>Избранное</span>
          </li>
        </router-link>

      </div>
      <telegram-login-temp mode="callback" telegram-login="myshatte_bot" @callback="onLogin" />
      <!-- TODO можно сделать что-то красивое -->
      <!-- <button v-if="store.isAuth" @click="store.logout()">EXIT</button> -->
    </ul>
  </header>
</template>

<style lang="scss" scoped></style>
