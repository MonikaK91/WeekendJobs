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
        <h2>Sign up</h2>
        <input type="email" v-model="email" name="email" placeholder="Email" />
        <input
          type="password"
          v-model="password"
          name="password"
          placeholder="Lozinka"
        />
        <input
          type="password"
          v-model="passwordRepeat"
          name="r_password"
          placeholder="Ponovi lozinku"
        />
        <button
          type="button"
          v-if="password != passwordRepeat"
          @click="checkpassword()"
          class="btnn"
        >
          Sign up
        </button>
        <!-- ako upisane lozinke nisu iste pozovi metodu checkpassword koja javlja upozorenje -->
        <button
          type="button"
          v-if="password == passwordRepeat"
          @click="signup()"
          class="btnn"
        >
          Sign up
        </button>
        <!-- ako su upisane lozinke iste pozovi metodu signup za registraciju -->
        <p class="separator">or</p>
        <button type="button" @click="signinGoogle()" class="btnnn">
          <img
            style="width: 20px; float: left; padding-left: 20px"
            src="https://img.icons8.com/color/48/000000/google-logo.png"
          />
          Continue with Google
        </button>

        <p class="link">
          Već imate račun?<br />
          <router-link to="/">Sign in</router-link> ovdje
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
  createUserWithEmailAndPassword,
  signInWithPopup,
  GoogleAuthProvider,
} from "firebase/auth";

export default {
  name: "signup",
  data() {
    return {
      email: "",
      password: "",
      passwordRepeat: "",
    };
  },
  methods: {
    // metoda za stvaranje novog korisničkog računa sa email adresom i lozinkom
    // prima podatke iz data funkcije
    signup() {
      const auth = getAuth();
      createUserWithEmailAndPassword(auth, this.email, this.password)
        .then(() => {
          console.log("Uspješna registracija");
        })
        .catch((error) => {
          console.error("Došlo je do greške", error);
          switch (error.code) {
            case "auth/invalid-email":
              alert("Nepravilna email adresa.");
              break;
            case "auth/weak-password":
              alert("Lozinka treba sadržavati najmanje 6 znakova");
              break;
            case "auth/email-already-in-use":
              alert("Email adresa se već upotrebljava");
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
    message() {
      alert("Prijavite se da bi mogli koristiti Weekend Jobs.");
    },
    checkpassword() {
      alert("Lozinke se ne podudaraju.");
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
  height: 450px;
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

.btnn {
  width: 240px;
  height: 40px;
  background: #fea501;
  border: none;
  margin-top: 30px;
  font-size: 18px;
  border-radius: 10px;
  cursor: pointer;
  color: #000;
  transition: 0.4s ease;
  font-weight: bold;
  text-decoration: none;
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
  font-weight: bold;
  text-decoration: none;
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
</style>
