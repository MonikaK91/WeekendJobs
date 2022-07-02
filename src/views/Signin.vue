<template>
  <div class="main">
    <div class="navbar">
      <div class="icon">
        <h2 class="logo">Weekend Jobs</h2>
      </div>

      <div class="menu">
        <ul>
          <li><a href="#" @click="message()">PONUDA POSLOVA</a></li>
          <li><a href="#" @click="message()">POTRAŽNJA POSLOVA</a></li>
          <li>
            <div class="dropdown">
              <a href="#" @click="message()">NAPIŠI OGLAS ▾</a>
              <div class="dropdown-content">
                <a href="#" @click="message()">Za ponudu</a>
                <a href="#" @click="message()">Za potražnju</a>
              </div>
            </div>
          </li>
        </ul>
      </div>
    </div>
    <div class="content">
      <h1>
        Tražiš posao <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ili<br />
        djelatnike vikendom?<br /><span>Weekend Jobs</span>
        <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ima baš ono što ti treba.
      </h1>
      <br />
      <p class="par">
        Pronađi pravi posao za sebe u bogatoj ponudi poslova <br />
        ili otkrij djelatnike baš po tvojoj mjeri.
      </p>
      <div class="form">
        <h2>Sign in</h2>
        <input type="email" v-model="email" name="email" placeholder="Email " />
        <input
          type="password"
          v-model="password"
          name="password"
          placeholder="Lozinka"
        />
        <a href="#popup1" class="popup-link">Zaboravili ste lozinku?</a>
        <div id="popup1" class="popup-container">
          <div class="popup-content">
            <a href="#" class="close">&times;</a>
            <h3>
              Molimo unesite email adresu koju ste koristili za registraciju i
              poslati ćemo link za resetiranje lozinke.
            </h3>
            <div class="form-group">
              <label for="email" class="label-title">Email</label>
              <input
                type="email"
                v-model="email2"
                id="email"
                class="form-input"
                required
              />
            </div>
            <button type="button" class="btn" @click="resetpassword()">
              Pošalji
            </button>
          </div>
        </div>
        <button type="submit" @click="signin()" class="btnn">Sign in</button>
        <p class="separator">or</p>
        <button type="button" @click="signinGoogle()" class="btnnn">
          <img
            style="width: 20px; float: left; padding-left: 20px"
            src="https://img.icons8.com/color/48/000000/google-logo.png"
          />
          Continue with Google
        </button>

        <p class="link">
          Nemate račun?<br />
          <router-link to="/signup">Sign up</router-link> ovdje
        </p>
      </div>
    </div>
    <div class="bottom">
      <a target="_blank" href="https://icons8.com/icon/17949/google"
        >Google icon by Icons8</a
      >
    </div>
  </div>
</template>

<script>
import { firebase } from "@/firebase";
import {
  getAuth,
  signInWithEmailAndPassword,
  signInWithPopup,
  GoogleAuthProvider,
  sendPasswordResetEmail,
} from "firebase/auth";

export default {
  name: "signin",
  data() {
    return {
      email: "",
      password: "",
      email2: "",
    };
  },
  methods: {
    // metoda za prijavu korisnika putem email-a i lozinke koji su već registrirani korisnici
    // prima podatke iz data funkcije
    signin() {
      const auth = getAuth();
      signInWithEmailAndPassword(auth, this.email, this.password)
        .then((result) => {
          console.log("Uspješna prijava");
        })
        .catch((error) => {
          console.error("Greška", error);
          switch (error.code) {
            case "auth/user-not-found":
              alert("Nepostojeći korisnik.");
              break;
            case "auth/wrong-password":
              alert("Kriva lozinka.");
              break;
            case "auth/invalid-email":
              alert("Nepravilna email adresa");
              break;
            default:
              alert("Dogodila se greška: " + error.message);
          }
        });
    },
    // alternativna metoda za prijavu putem svog Google računa
    signinGoogle() {
      const provider = new GoogleAuthProvider();
      // izrada instance objekta Google provider
      const auth = getAuth();
      signInWithPopup(auth, provider)
        .then((result) => {
          console.log(result);
        })
        .catch((error) => {
          console.log(error);
        });
    },
    // metoda za resetiranje lozinke ako ju je korisnik zaboravio
    // prima podatak iz data funkcije
    resetpassword() {
      const auth = getAuth();
      sendPasswordResetEmail(auth, this.email2)
        .then(() => {
          console.log("Poslan e-mail");
          alert("Poslan link na email adresu.");
          this.email2 = null;
          // nakon što je poslan email prazni se input polje unutar popup-a
        })
        .catch((error) => {
          console.log(error);
          if (error.code == "auth/missing-email") {
            alert("Moraš upisati email adresu");
          } else {
            alert("Dogodila se greška: ", error.message);
          }
        });
    },
    message() {
      alert("Prijavite se da bi mogli koristiti Weekend Jobs.");
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
    url("~@/assets/4.jpg");
  background-position: center;
  background-size: cover;
  height: 100vh;
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
  padding-top: 10px;
  float: left;
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

.content {
  width: 1200px;
  height: auto;
  margin: auto;
  color: #fff;
  position: relative;
}

.content .par {
  padding-left: 20px;
  padding-bottom: 25px;
  font-family: Arial;
  letter-spacing: 1.5px;
  line-height: 30px;
  font-size: 22px;
}

.content h1 {
  font-family: "Times New Roman";
  font-size: 40px;
  padding-left: 10px;
  margin-top: 9%;
  letter-spacing: 3px;
}

.content span {
  color: #fea501;
  font-size: 50px;
  text-shadow: 3px 3px #000;
}

.form {
  width: 250px;
  height: 400px;
  background: linear-gradient(
    to top,
    rgba(0, 0, 0, 0.8) 50%,
    rgba(0, 0, 0, 0.8) 50%
  );
  position: absolute;
  top: -20px;
  left: 870px;
  transform: translate(0%, -5%);
  border-radius: 10px;
  padding: 25px;
}

.form h2 {
  width: 220px;
  font-family: sans-serif;
  text-align: center;
  color: #fea501;
  font-size: 22px;
  margin: 2px;
  padding: 8px;
}

.form input {
  width: 240px;
  height: 35px;
  background: transparent;
  border-bottom: 1px solid #fea501;
  border-top: none;
  border-right: none;
  border-left: none;
  color: #fff;
  font-size: 15px;
  letter-spacing: 1px;
  margin-top: 30px;
  font-family: sans-serif;
}

.form input:focus {
  outline: none;
}

::placeholder {
  color: #fff;
  font-family: Arial;
}

::-ms-reveal {
  filter: invert(100%);
}

.l:hover {
  color: #fea501;
}

.btnn {
  width: 240px;
  height: 40px;
  background: #fea501;
  border: none;
  margin-top: 20px;
  font-size: 18px;
  border-radius: 10px;
  cursor: pointer;
  color: #000;
  transition: 0.4s ease;
  text-decoration: none;
  font-weight: bold;
}

.btnn:hover {
  background: #fff;
  color: #000;
}

.form .separator {
  color: #fff;
  text-align: center;
  padding: 10px 0 10px 0;
  font-family: Arial;
  font-size: 13px;
}

.btnnn {
  width: 240px;
  height: 40px;
  background: #fff;
  border: none;
  font-size: 16px;
  border-radius: 10px;
  cursor: pointer;
  color: #000;
  transition: 0.4s ease;
  text-decoration: none;
  font-weight: bold;
}

.form .link {
  font-family: Arial, Helvetica, sans-serif;
  font-size: 17px;
  padding-top: 20px;
  text-align: center;
}

.form .link a {
  text-decoration: none;
  color: #fea501;
}

.bottom {
  position: absolute;
  bottom: 0;
  right: 0;
}

.bottom a {
  color: #fff;
  text-decoration: none;
}

.bottom a:hover {
  color: #fea501;
}

.form .popup-link {
  text-decoration: none;
  color: #fff;
  margin-top: 10px;
  float: right;
  font-size: 14px;
}

.form .popup-container {
  visibility: hidden;
  opacity: 0;
  transition: all 0.3s ease-in-out;
  transform: scale(1.3);
  position: fixed;
  z-index: 1;
  right: 70px;
  top: 0;
  width: 1000px;
  height: 300px;
  display: flex;
  align-items: center;
}

.form .popup-content {
  background-color: #fefefe;
  margin: auto;
  padding: 20px;
  border: 1px solid #888;
  width: 100%;
}

.form .popup-content a.close {
  color: #aaaaaa;
  float: right;
  font-size: 28px;
  font-weight: bold;
  background: none;
  padding: 0;
  margin: 0;
  text-decoration: none;
}

.form .popup-content a.close:hover {
  color: #333;
}

.form .popup-container:target {
  visibility: visible;
  opacity: 1;
  transform: scale(1);
}

.form .popup-container h3 {
  margin: 10px;
  color: #fea501;
}

.form .form-group {
  margin: 10px;
}

.form .label-title {
  color: #000;
  font-size: 15px;
  font-weight: bold;
}

.form .form-input {
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
  margin-top: 10px;
}

.form .btn {
  display: inline-block;
  padding: 10px 20px;
  background-color: #fea501;
  font-size: 17px;
  border: none;
  border-radius: 5px;
  color: #000;
  cursor: pointer;
  margin: 10px;
}

.form .btn:hover {
  background-color: #fff;
  color: #000;
  border: 1px;
  border-style: solid;
  border-color: #fea501;
}
</style>
