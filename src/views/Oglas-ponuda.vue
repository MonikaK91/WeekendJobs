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
                <router-link to="/potraznja">POTRAŽNJA POSLOVA</router-link>
              </li>
              <li>
                <div class="dropdown">
                  <a href="#" style="text-decoration: underline #fea501 4px"
                    >NAPIŠI OGLAS ▾</a
                  >
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
    <form class="ad-form" @submit.prevent="postNewAd">
      <div class="form-header">
        <h1>
          Objavi oglas za <br />
          ponudu
        </h1>
      </div>
      <div class="form-body">
        <h3>Radno mjesto:</h3>
        <br />
        <div class="form-group">
          <label for="nazivposla" class="label-title">Naziv posla</label>
          <input
            v-model="nazivPosla"
            type="text"
            id="nazivposla"
            class="form-input"
            required
          />
        </div>
        <div class="form-group">
          <label for="mjestorada" class="label-title">Mjesto rada</label>
          <input
            v-model="mjestoRada"
            type="text"
            id="mjestorada"
            class="form-input"
            required
          />
        </div>
        <div class="form-group">
          <label for="vrijeme" class="label-title">Radno vrijeme</label>
          <select
            v-model="radnoVrijeme"
            name="radnovrijeme"
            id="vrijeme"
            class="form-input"
            required
          >
            <option value="Puno radno vrijeme">Puno radno vrijeme</option>
            <option value="Nepuno radno vrijeme">Nepuno radno vrijeme</option>
            <option value="Skraceno radno vrijeme">
              Skraćeno radno vrijeme
            </option>
          </select>
        </div>
        <div class="form-group">
          <label for="datum" class="label-title">Datum isteka natječaja</label>
          <input
            v-model="datum"
            type="date"
            id="datum"
            class="form-input"
            name="date"
            required
          />
        </div>
        <h3>Posloprimac:</h3>
        <br />
        <div class="form-group">
          <label for="obrazovanje" class="label-title"
            >Razina obrazovanja</label
          >
          <input
            v-model="obrazovanje"
            type="text"
            id="obrazovanje"
            class="form-input"
            required
          />
        </div>
        <div class="form-group">
          <label for="iskustvo" class="label-title">Radno iskustvo</label>
          <input
            v-model="iskustvo"
            type="text"
            id="iskustvo"
            class="form-input"
            required
          />
        </div>
        <div class="form-group">
          <label for="opis" class="label-title">Opis posla</label>
          <textarea
            v-model="opisPosla"
            id="opis"
            class="form-input"
            rows="4"
            cols="50"
            style="height: auto"
            maxlength="100"
            required
          ></textarea>
          <p>dozvoljeno 100 znakova</p>
        </div>
        <div class="form-group">
          <label for="informacije" class="label-title">Ostale infomacije</label>
          <textarea
            v-model="informacije"
            id="informacije"
            class="form-input"
            rows="4"
            cols="50"
            style="height: auto"
            maxlength="100"
            required
          ></textarea>
          <p>dozvoljeno 100 znakova</p>
        </div>
        <h3>Poslodavac:</h3>
        <br />
        <div class="form-group">
          <label for="poslodavac" class="label-title">Poslodavac</label>
          <input
            v-model="poslodavac"
            type="text"
            id="poslodavac"
            class="form-input"
            required
          />
        </div>
        <div class="form-group">
          <label for="phone" class="label-title">Telefon</label>
          <input
            v-model="telefon"
            type="tel"
            id="phone"
            class="form-input"
            required
          />
        </div>
      </div>
      <div class="form-footer">
        <span>* sva polja su obavezna</span>
        <button type="submit" class="btn">Kreiraj</button>
      </div>
    </form>
  </div>
</template>

<script>
import { firebase, db } from "@/firebase";
import { getAuth, signOut } from "firebase/auth";
import store from "@/store";
import { collection, addDoc } from "firebase/firestore";

export default {
  name: "oglas-ponuda",
  data: function () {
    return {
      nazivPosla: "",
      mjestoRada: "",
      radnoVrijeme: "",
      datum: "",
      obrazovanje: "",
      iskustvo: "",
      opisPosla: "",
      informacije: "",
      poslodavac: "",
      telefon: "",
    };
  },
  methods: {
    // metoda za stvaranje novog dokumenta na firestore-u u kolekciji oglasiPonuda
    async postNewAd() {
      const docRef = await addDoc(collection(db, "oglasiPonuda"), {
        // spremamo upisana polja iz forme u dokument
        job: this.nazivPosla,
        workPlace: this.mjestoRada,
        workingTime: this.radnoVrijeme,
        date: this.datum,
        education: this.obrazovanje,
        experience: this.iskustvo,
        description: this.opisPosla,
        information: this.informacije,
        employer: this.poslodavac,
        phone: this.telefon,
        email: store.currentUser,
        posted_at: Date.now(),
      });
      console.log("Spremljeno.");
      this.nazivPosla = "";
      this.mjestoRada = "";
      this.radnoVrijeme = "";
      this.datum = "";
      this.obrazovanje = "";
      this.iskustvo = "";
      this.opisPosla = "";
      this.informacije = "";
      this.poslodavac = "";
      this.telefon = "";
      // praznimo sva polja
      alert("Oglas je uspješno kreiran.");
      this.$router.replace({ name: "Ponuda" });
      // nakon što je napravljen oglas prebacuje nas na stranicu ponude
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

.ad-form {
  font-family: Arial;
  width: 650px;
  margin: 80px auto;
  background: #fff;
  border-radius: 10px;
  border: 5px;
  border-style: solid;
  border-color: #fea501;
  box-shadow: 10px 10px 5px #fea501;
}

.form-header {
  background-color: #eff0f1;
  border-top-left-radius: 5px;
  border-top-right-radius: 5px;
}

.form-header h1 {
  font-size: 30px;
  text-align: center;
  color: #000;
  padding: 20px 0;
  border-bottom: 1px solid #cccccc;
  margin: 0px;
}

.form-body {
  padding: 10px 40px;
}

.form-body h3 {
  color: #fea501;
}

.form-group {
  margin-bottom: 20px;
}

.form-body .label-title {
  color: #000;
  font-size: 17px;
  font-weight: bold;
}

.form-body .form-input {
  font-size: 17px;
  box-sizing: border-box;
  width: 100%;
  height: 34px;
  padding-left: 10px;
  padding-right: 10px;
  color: #333333;
  text-align: left;
  border: 1px solid #d6d6d6;
  border-radius: 4px;
  background: #fff;
  outline: none;
}

::-webkit-input-placeholder {
  color: #d9d9d9;
}

.form-body p {
  font-size: 13px;
  font-family: Arial;
  font-style: italic;
  float: right;
}

.form-footer {
  clear: both;
}

.ad-form .form-footer {
  background-color: #eff0f1;
  border-bottom-left-radius: 10px;
  border-bottom-right-radius: 10px;
  padding: 10px 4px;
  text-align: right;
  border-top: 1px solid #cccccc;
}

.form-footer span {
  float: left;
  margin-top: 10px;
  color: #999;
  font-style: italic;
  font-weight: thin;
}

.btn {
  display: inline-block;
  padding: 10px 20px;
  background-color: #fea501;
  font-size: 17px;
  border: none;
  border-radius: 5px;
  color: #000;
  cursor: pointer;
}

.btn:hover {
  background-color: #fff;
  color: #000;
  border: 1px;
  border-style: solid;
  border-color: #fea501;
}
</style>
