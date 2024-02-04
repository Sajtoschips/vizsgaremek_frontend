<template>
  <nav class="navbar navbar-expand-lg navbar-light bg-light">
    <div class="container">
      <a class="navbar-brand" href="/"><ion-icon name="home-outline"></ion-icon></a>
      <button
        class="navbar-toggler"
        type="button"
        data-bs-toggle="collapse"
        data-bs-target="#navbarNavDropdown"
        aria-controls="navbarNavDropdown"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarNavDropdown">
        <div class="navbar-nav col-md-6">
          <ul class="navbar-nav me-auto mb-2 mb-lg-0">
            <div v-for="menu in menuItems">
              <li class="nav-item" v-if="menu.isLoggedIn">
                <router-link class="nav-link" :to="menu.to">{{
                  menu.title
                }}</router-link>
              </li>
            </div>
          </ul>
        </div>

        <div class="navbar-nav col-md-6">
          <ul class="navbar-nav ms-auto mb-2 mb-lg-0">
            <li class="nav-item" v-if="!status.loggedIn">
              <div class="dropdown">
                <a
                  
                  id="navbarDropdown"
                  role="button"
                  data-bs-toggle="dropdown"
                  aria-expanded="false"
                ><ion-icon name="person-outline"></ion-icon></a>
                <ul class="dropdown-menu" aria-labelledby="navbarDropdown">
                  <li>
                    <router-link class="dropdown-item" to="/bejelentkezes"
                      >Bejelentkezés</router-link
                    >
                  </li>
                  <li>
                    <router-link class="dropdown-item" to="/regisztracio"
                      >Regisztráció</router-link
                    >
                  </li>
                  <li>
                    <div class="form-check form-switch">
                      <input
                        class="form-check-input"
                        type="checkbox"
                        id="darkSwitch"
                        :checked="darkMode"
                        @change="toggleDarkMode"
                      />
                      <label class="form-check-label" for="darkSwitch"
                        >Dark Mode</label
                      >
                    </div>
                  </li>
                </ul>
              </div>
            </li>
            <li class="nav-item" v-if="status.loggedIn">
              <a class="nav-link" href="#" @click="onLogout">Kijelentkezés</a>
            </li>

            <div v-if="status.loggedIn">
              {{ user.name }}
            </div>
          </ul>
        </div>
      </div>
    </div>
  </nav>
</template>

<script setup>
import { computed } from "vue";
import { useUserStore } from "../stores/userstore";
import { storeToRefs } from "pinia";
import { useRouter } from "vue-router";
import { ref } from "vue";
const { status, user } = storeToRefs(useUserStore());
const { logout } = useUserStore();
const router = useRouter();

const menuItems = computed(() => {
  return [
    {
      title: "Nyitó oldal",
      to: "/",
      isLoggedIn: true,
    },
    {
      title: "Termékek",
      to: "/termekek",
      isLoggedIn: true,
    },
    {
      title: "Új blog",
      to: "/ujblog",
      isLoggedIn: status.value.loggedIn,
    },
  ];
});

function onLogout() {
  logout().then(() => {
    router.push("/");
  });
}

defineProps({
  darkMode: Boolean,
});

const darkMode = ref(false);

function toggleDarkMode() {
  darkMode.value = !darkMode.value;
  emit("update:darkMode", darkMode.value);
}
</script>

<style lang="scss" scoped>

</style>
