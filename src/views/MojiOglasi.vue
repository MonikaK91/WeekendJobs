<template>
  <div>
    <header>
      <div class="main">
        <div class="navbar">
          <div class="icon">
            <router-link to="/pocetna"
              ><h2 class="logo">Weekend Jobs</h2></router-link
            >
          </div>

          <div class="menu">
            <ul>
              <li>
                <router-link to="/ponuda">PONUDA POSLOVA</router-link>
              </li>
              <li>
                <router-link to="/potraznja">POTRAŽNJA POSLOVA</router-link>
              </li>
              <li>
                <div class="dropdown">
                  <a href="#">NAPIŠI OGLAS ▾</a>
                  <div class="dropdown-content">
                    <router-link to="/oglas-ponuda">Za ponudu</router-link>
                    <router-link to="/oglas-potraznja"
                      >Za potražnju</router-link
                    >
                  </div>
                </div>
              </li>
              <li>
                <div class="dropdown">
                  <a href="#"
                    ><a style="font-size: 25px" href="#" @click="logout()"
                      ><ion-icon name="power"></ion-icon></a
                  ></a>
                  <div class="dropdown-content2">
                    <router-link to="/moji-oglasi">Moji oglasi</router-link>
                  </div>
                </div>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </header>
    <div class="headline">
      <h1 class="caption"><span>Moji oglasi</span></h1>
    </div>
    <p v-if="this.cards == '' && this.cards2 == ''">Nema još oglasa</p>
    <!-- ako korisnik nema ni oglase ponude ni potražnje onda se pojavljuje tekst iznad-->
    <kartica-ponuda v-for="card in cards" :key="card.id" :info="card" />
    <!-- za prikaz oglasa ponude -->
    <kartica-potraznja v-for="card in cards2" :key="card.id" :info="card" />
    <!-- za prikaz oglasa potraznje -->
  </div>
</template>

<script>
import KarticaPonuda from "@/components/KarticaPonuda.vue";
import KarticaPotraznja from "@/components/KarticaPotraznja.vue";
import { firebase, db } from "@/firebase";
import { getAuth, signOut } from "firebase/auth";
import {
  collection,
  getDocs,
  QuerySnapshot,
  query,
  orderBy,
  doc,
} from "firebase/firestore";

import store from "@/store";

export default {
  name: "mojiOglasi",
  data() {
    return {
      cards: [],
      cards2: [],
    };
  },
  async mounted() {
    this.getAdsPonuda();
    this.getAdsPotraznja();
  },
  components: {
    KarticaPonuda,
    KarticaPotraznja,
  },
  methods: {
    // metoda za dohvaćanje svih oglasa odnosno dokumenta sa firestore-a iz kolekcije oglasiPonuda koji se tiču jednog korisnika
    async getAdsPonuda() {
      const querySnapshot = await getDocs(
        query(collection(db, "oglasiPonuda"), orderBy("posted_at", "desc"))
      );
      querySnapshot.forEach((doc) => {
        const data = doc.data();
        if (data.email == store.currentUser)
          // punimo cards pomoću push sa potrebnim podacima
          // ako je autor oglasa isti kao i trenutni korisnik aplikacije
          this.cards.push({
            id: doc.id,
            job: data.job,
            workPlace: data.workPlace,
            workingTime: data.workingTime,
            date: data.date,
            education: data.education,
            experience: data.experience,
            description: data.description,
            information: data.information,
            employer: data.employer,
            phone: data.phone,
            email: data.email,
          });
      });
    },
    // metoda za dohvaćanje svih oglasa odnosno dokumenta sa firestore-a iz kolekcije oglasiPotraznja koji se tiču jednog korisnika
    async getAdsPotraznja() {
      const querySnapshot = await getDocs(
        query(collection(db, "oglasiPotraznja"), orderBy("posted_at", "desc"))
      );
      querySnapshot.forEach((doc) => {
        const data = doc.data();
        if (data.e_mail == store.currentUser)
          // punimo cards2 pomoću push sa potrebnim podacima
          // ako je autor oglasa isti kao i trenutni korisnik aplikacije
          this.cards2.push({
            id: doc.id,
            image: data.image,
            firstName: data.name,
            lastName: data.lastName,
            email: data.email,
            phone: data.phone,
            job: data.job,
            city: data.city,
            message: data.message,
            e_mail: data.e_mail,
          });
      });
    },
    // metoda za odjavu korisnika
    logout() {
      const auth = getAuth();
      signOut(auth)
        .then(() => {
          console.log("Odjava");
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
};
</script>

<style scoped lang="scss">
* {
  margin: 0;
  padding: 0;
}

.main {
  width: 100%;
  background: linear-gradient(
      to top,
      rgba(0, 0, 0, 0.5) 50%,
      rgba(0, 0, 0, 0.5) 50%
    ),
    url("~@/assets/13.jpg");
  background-position: center;
  background-size: cover;
  height: 50vh;
  border-bottom: solid 5px #000;
}

.navbar {
  width: 1300px;
  height: 75px;
  margin: auto;
}

.icon {
  width: 270px;
  float: left;
  height: 70px;
}

.logo {
  color: #fea501;
  font-size: 35px;
  font-family: Arial;
  padding-left: 20px;
  float: left;
  padding-top: 10px;
  margin-top: 5px;
  text-shadow: 4px 4px #000;
}

.menu {
  width: 400px;
  float: left;
  height: 70px;
}

ul {
  float: left;
  display: -webkit-box;
  justify-content: center;
  align-items: center;
}

ul li {
  list-style: none;
  margin-left: 150px;
  margin-top: 27px;
  font-size: 15px;
}

ul li a {
  text-decoration: none;
  color: #fff;
  font-family: Arial;
  font-weight: bold;
  transition: 0.4s ease-in-out;
}

ul li a:hover {
  color: #fea501;
}

.dropdown {
  position: relative;
  display: inline-block;
}

.dropdown-content {
  position: absolute;
  background-color: #fff;
  z-index: 98;
  max-height: 0;
  min-width: 160px;
  transition: max-height 0.15s ease-out;
  overflow: hidden;
  border-radius: 5px;
  opacity: 0.6;
}

.dropdown-content a {
  color: #000;
  background-color: #fff;
  padding: 12px 16px;
  text-decoration: none;
  display: block;
}

.dropdown:hover .dropdown-content {
  max-height: 500px;
  min-width: 160px;
  transition: max-height 0.25s ease-in;
}

.dropdown-content2 {
  position: absolute;
  background-color: #fff;
  z-index: 98;
  max-height: 0;
  min-width: 90px;
  transition: max-height 0.15s ease-out;
  overflow: hidden;
  border-radius: 5px;
  opacity: 0.6;
}

.dropdown-content2 a {
  color: #000;
  background-color: #fff;
  padding: 12px 16px;
  text-decoration: none;
  display: block;
}

.dropdown:hover .dropdown-content2 {
  max-height: 200px;
  min-width: 100px;
  transition: max-height 0.25s ease-in;
}

.headline {
  margin: 100px;
}

.caption {
  width: 40%;
  text-align: center;
  font-family: Arial;
  font-size: 40px;
  border-bottom: 1px solid #000;
  line-height: 0.1em;
  margin: 10px 0 20px;
  margin-left: auto;
  margin-right: auto;
  color: #fea501;
}

.caption span {
  background: #fff;
  padding: 0 10px;
}

p {
  text-align: center;
  font-size: 20px;
  font-family: Arial;
}
</style>
