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
          potražnju
        </h1>
      </div>
      <div class="form-body">
        <div class="form-group">
          <croppa
            :width="200"
            :height="200"
            :placeholder="'Odaberi sliku'"
            :placeholder-font-size="16"
            :initial-image="require('@/assets/no-image1.png')"
            v-model="slika"
          ></croppa>
          <p>
            ako ne želiš staviti <br />svoju sliku ostavi <br />već odabranu
          </p>
        </div>

        <div class="horizontal-group">
          <div class="form-group left">
            <label for="ime" class="label-title">Ime</label>
            <input
              v-model="ime"
              type="text"
              id="ime"
              class="form-input"
              required
            />
          </div>
          <div class="form-group right">
            <label for="prezime" class="label-title">Prezime</label>
            <input
              v-model="prezime"
              type="text"
              id="prezime"
              class="form-input"
              required
            />
          </div>
        </div>
        <div class="form-group">
          <label for="email" class="label-title">Email</label>
          <input
            v-model="email"
            type="email"
            id="email"
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
        <div class="form-group">
          <label for="posao" class="label-title">Naziv posla</label>
          <input
            v-model="posao"
            type="text"
            id="posao"
            class="form-input"
            required
          />
        </div>
        <div class="form-group">
          <label for="grad" class="label-title">Grad</label>
          <input
            v-model="grad"
            type="text"
            id="grad"
            class="form-input"
            required
          />
        </div>
        <div class="form-group">
          <label for="poruka" class="label-title">Poruka</label>
          <textarea
            v-model="poruka"
            id="poruka"
            class="form-input"
            rows="5"
            cols="50"
            style="height: auto"
            maxlength="400"
            required
          ></textarea>
          <p>dozvoljeno 400 znakova</p>
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
import { firebase, db, storage } from "@/firebase";
import { getAuth, signOut } from "firebase/auth";
import store from "@/store";
import { collection, addDoc } from "firebase/firestore";
import {
  getStorage,
  ref,
  uploadBytesResumable,
  getDownloadURL,
} from "firebase/storage";

export default {
  name: "oglas-potraznja",
  data: function () {
    return {
      slika: null,
      ime: "",
      prezime: "",
      email: "",
      telefon: "",
      posao: "",
      grad: "",
      poruka: "",
    };
  },
  methods: {
    // metoda za dohvaćanje bajtova iz slike koju je postavio korisnik pomoću metode generateBlob()
    getImage() {
      return new Promise((resolveFn, errorFn) => {
        this.slika.generateBlob((data) => {
          resolveFn(data);
        });
      });
    },
    // metoda za stvaranje novog dokumenta na firestore-u u kolekciji oglasiPotraznja
    async postNewAd() {
      try {
        let blobData = await this.getImage();
        // poziv getImage metode
        let imageName =
          "posts/" + store.currentUser + "/" + Date.now() + ".png";
        // naziv slike, te datoteke u kojima će se nalaziti slike

        const storage = getStorage();
        // referenca storage-a koja se koristi za stvaranje referenci u našem storage-u
        const storageRef = ref(storage, imageName);
        // referenca na imageName odnosno sliku
        const uploadTask = await uploadBytesResumable(storageRef, blobData);
        // učitavamo blobData. Metoda za prijenos Blob datoteke
        let url2 = await getDownloadURL(storageRef);
        // dobijemo javni link slike
        console.log("Javni link", url2);
        const docRef = await addDoc(collection(db, "oglasiPotraznja"), {
          // spremamo upisana polja iz forme i sliku u dokument
          image: url2,
          name: this.ime,
          lastName: this.prezime,
          email: this.email,
          phone: this.telefon,
          job: this.posao,
          city: this.grad,
          message: this.poruka,
          e_mail: store.currentUser,
          posted_at: Date.now(),
        });
        console.log("Spremljeno. ");
        this.slika.remove();
        this.ime = "";
        this.prezime = "";
        this.email = "";
        this.telefon = "";
        this.posao = "";
        this.grad = "";
        this.poruka = "";
        // praznimo sva polja
        alert("Oglas je uspješno kreiran.");
        this.$router.replace({ name: "Potraznja" });
        // nakon što je napravljen oglas prebacuje nas na stranicu potražnje
      } catch (e) {
        console.error("Greška", e);
      }
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

.horizontal-group .left {
  float: left;
  width: 49%;
}

.horizontal-group .right {
  float: right;
  width: 49%;
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
