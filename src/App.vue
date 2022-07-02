<template>
  <div id="app">
    <!--<div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </div> -->
    <router-view />
  </div>
</template>

<script>
import { firebase } from "@/firebase";
import { getAuth, onAuthStateChanged } from "firebase/auth";
import router from "@/router";
import store from "@/store";

// metoda za dobivanje trenutnog korisnika
// postavlja se observer (promatrač) na auth objekt
// koristeći observer osiguravamo da auth objekt nije u srednjem stanju (npr. inicijalizacija)
const auth = getAuth();
onAuthStateChanged(auth, (user) => {
  const currentRoute = router.currentRoute;

  if (user) {
    // korisnik je prijavljen
    console.log(user.email);
    store.currentUser = user.email;
    if (!currentRoute.meta.needsUser) {
      router.replace({ name: "Pocetna" });
      // ako trenutna ruta ne treba korisnika onda nas prebacuje na početnu stranicu
    }
  } else {
    // korisnik nije prijavljen
    console.log("No user");
    store.currentUser = null;
    if (currentRoute.meta.needsUser) {
      router.replace({ name: "Signin" });
      // ako trenutna ruta treba korisnika onda nas prebacuje na signin stranicu
    }
  }
});

export default {
  name: "app",
};
</script>

<style lang="scss"></style>
