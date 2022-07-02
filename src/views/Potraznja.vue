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
              <li><router-link to="/ponuda">PONUDA POSLOVA</router-link></li>
              <li>
                <router-link
                  to="/potraznja"
                  style="text-decoration: underline #fea501 4px"
                  >POTRAŽNJA POSLOVA</router-link
                >
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
    <div class="search">
      <input
        v-model="pojam"
        class="srch"
        type="search"
        name="search"
        placeholder="Pretraži po zanimanju ili gradu"
      />
      <a href="#"
        ><button type="button" class="btn" @click="getFilteredAds()">
          Pretraži
        </button></a
      >
    </div>
    <div class="clear">
      <a href="#"
        ><button
          type="button"
          v-if="cards2 != ''"
          class="btnn"
          @click="clear()"
        >
          Očisti dosadašnje pretraživanje
        </button></a
      >
    </div>
    <!-- ako cards2 nije prazan pojavljuje je button očisti i onda se može pozvati metoda clear() -->
    <kartica-potraznja v-for="card in cards" :key="card.id" :info="card" />
    <!-- za sve oglase potražnje -->
    <kartica-potraznja v-for="card in cards2" :key="card.id" :info="card" />
    <!-- za filtrirane oglase potražnje -->

    <!-- v-for direktiva jer želimo ponavljati kartice potražnje 
    card in cards - ponavljaj karticu potražnje za svaki card u cards
    :key - označava neki jedinstveni element za svaki card (oglas)
    :info - prenosi vrijednost -->
  </div>
</template>

<script>
import KarticaPotraznja from "@/components/KarticaPotraznja.vue";
import { firebase, db } from "@/firebase";
import { getAuth, signOut } from "firebase/auth";
import { collection, getDocs, query, orderBy, limit } from "firebase/firestore";

export default {
  name: "potraznja",
  data() {
    return {
      cards: [],
      cards2: [],
      pojam: "",
    };
  },
  async mounted() {
    this.getAds();
  },
  components: {
    KarticaPotraznja,
  },
  methods: {
    // metoda za dohvaćanje svih oglasa odnosno dokumenta sa firestore-a iz kolekcije oglasiPotraznja
    async getAds() {
      const querySnapshot = await getDocs(
        query(collection(db, "oglasiPotraznja"), orderBy("posted_at", "desc"))
      );
      querySnapshot.forEach((doc) => {
        const data = doc.data();
        this.cards.push({
          // punimo cards pomoću push sa potrebnim podacima
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
    // metoda za brisanje pretraživanja
    // prvo prazni cards2, a onda poziva getAds() metodu za prikaz svih oglasa
    async clear() {
      this.cards2 = [];
      this.getAds();
    },
    // metoda za dohvaćanje filtriranih oglasa
    async getFilteredAds() {
      this.cards = [];
      // prvo praznimo cards da se oglasi ne bi ponavljali i da bi se prikazivali samo filtrirani oglasi
      let term = this.pojam.toLowerCase();
      // upisani pojam je pretvoren u mala slova da veličina slova ne bi bila bitna u pretraživanju
      const querySnapshot = await getDocs(
        query(collection(db, "oglasiPotraznja"), orderBy("posted_at", "desc"))
      );
      querySnapshot.forEach((doc) => {
        const data = doc.data();
        if (
          data.job.toLowerCase().includes(term) ||
          data.city.toLowerCase().includes(term)
        )
          // provjeravamo da li se upisani pojam nalazi u bazi
          // ovdje možemo pretraživati ili po poslu ili po gradu
          // te punimo cards2 sa potrebnim podacima
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
          });
      });
      this.pojam = null;
      // praznimo input polje za pretraživanje
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

.search {
  width: 1000px;
  margin-top: 60px;
  margin-bottom: 60px;
  margin-left: 518px;
}

.srch {
  font-family: "Times New Roman";
  width: 400px;
  height: 54px;
  background: transparent;
  border: 2px solid #fea501;
  margin-top: 13px;
  color: #000;
  border-right: none;
  font-size: 18px;
  float: left;
  padding: 10px;
  border-bottom-left-radius: 5px;
  border-top-left-radius: 5px;
}

.btn {
  width: 100px;
  height: 54px;
  background: #fea501;
  border: 3px solid #fea501;
  margin-top: 13px;
  color: #fff;
  font-size: 16px;
  border-bottom-right-radius: 5px;
  border-top-right-radius: 5px;
  transition: 0.2s ease;
  cursor: pointer;
}

.btn:hover {
  color: #000;
}

.btn:focus {
  outline: none;
}

.srch:focus {
  outline: none;
}

.clear {
  width: 900px;
  margin-bottom: 20px;
  margin-left: 615px;
}

.btnn {
  width: 300px;
  height: 54px;
  background: #fea501;
  border: 3px solid #fea501;
  color: #fff;
  font-size: 16px;
  border-radius: 5px;
  transition: 0.2s ease;
  cursor: pointer;
}

.btnn:hover {
  color: #000;
}

.btnn:focus {
  outline: none;
}
</style>
