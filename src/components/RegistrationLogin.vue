<template>
  <div class="py-44 bg-slate-100">
    <div class="container mx-auto">
      <div class="flex items-center justify-between">
        <div class="w-1/2">
          <img
            class="w-1/2 mb-5"
            src="https://upload.wikimedia.org/wikipedia/commons/thumb/8/89/Facebook_Logo_%282019%29.svg/1200px-Facebook_Logo_%282019%29.svg.png"
            alt=""
          />
          <p class="text-2xl font-medium">Facebook помогает вам всегда оставаться на связи и общаться со своими знакомыми.</p>
        </div>
        <div class="w-1/3 bg-white rounded-xl p-5">
          <div v-if="setForm === 1">
            <input class="w-full p-3 rounded-lg border my-2" placeholder="Электронный адрес" type="text">
            <input class="w-full p-3 rounded-lg border my-2" placeholder="Пароль" type="text">
            <button class="bg-main p-3 text-xl font-medium mt-2 text-center w-full rounded-lg text-white">Вход</button>
          </div>
          <div v-if="setForm === 2">
            <input class="w-full p-3 rounded-lg border my-2" placeholder="Имя" type="text">
            <input class="w-full p-3 rounded-lg border my-2" placeholder="Фамилия" type="text">
            <input class="w-full p-3 rounded-lg border my-2" placeholder="Email" type="text">
            <input class="w-full p-3 rounded-lg border my-2" placeholder="Телефон" type="text">
            <div class="flex items-center justify-between">
              <div>
                <label class="mr-2" for="">Мужчина</label>
                <input v-model="registration.gender" type="radio" name="gender" value="male">
              </div>
              <div>
                <label class="mr-2" for="">Женщина</label>
                <input v-model="registration.gender" type="radio" name="gender" value="female">
              </div>
            </div>
            <input class="w-full p-3 rounded-lg border my-2" placeholder="Пароль" type="text">
            <button class="bg-main p-3 text-xl font-medium mt-2 text-center w-full rounded-lg text-white">Регистрация</button>
          </div>
          <div class="w-full border-b my-2"></div>
          <button @click="setForm = 2" v-if="setForm === 1" class="bg-green-500 p-3 text-xl font-medium text-center w-full rounded-lg text-white">Регистрация</button>
          <button @click="setForm = 1" v-if="setForm === 2" class="bg-green-500 p-3 text-xl font-medium text-center w-full rounded-lg text-white">Вход</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "RegistrationLogin",
  data() {
    return {
      setForm: 1,
      registration: {
        name: null,
        surname: null,
        email: null,
        phone: null,
        gender: null,
        password: null,
      }
    }
  }
};
</script>