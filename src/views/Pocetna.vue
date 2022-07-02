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
      <h1 class="caption"><span>Naše usluge</span></h1>
    </div>
    <div class="container">
      <div class="card-container">
        <div class="header">
          <img src="../assets/10.jpg" alt="office work" />
        </div>
        <div class="description">
          <h1>Neka te poslovi pronađu</h1>
          <h5>
            Izradi svoj besplatni oglas kako bi dobio pozive za intervjue
            poslova koji su za tebe.
          </h5>
          <button class="btn">
            <router-link to="/oglas-potraznja">Kreni</router-link>
          </button>
        </div>
      </div>
    </div>
    <div class="container">
      <div class="card-container">
        <div class="description">
          <h1>Pronađi pravi posao u kojem ćeš uživati</h1>
          <h5>Pretraži našu bogatu ponudu poslova.</h5>
          <button class="btn">
            <router-link to="/ponuda">Kreni</router-link>
          </button>
        </div>
        <div class="header">
          <img src="../assets/1.jpg" alt="bartender" />
        </div>
      </div>
    </div>
    <div class="container">
      <div class="card-container">
        <div class="header">
          <img src="../assets/21.jpg" alt="farmer" />
        </div>
        <div class="description">
          <h1>
            Otkrij savršenog kandidata koji će ti pomoći u tvom poslovanju
          </h1>
          <h5>Prelistaj širok izbor potencijalnih zaposlenika.</h5>
          <button class="btn">
            <router-link to="/potraznja">Kreni</router-link>
          </button>
        </div>
      </div>
    </div>
    <div class="container">
      <div class="card-container">
        <div class="description">
          <h1>Brz i lak pronalazak idealnog djelatnika</h1>
          <h5>
            Kreiraj oglas za vlastiti posao kako bi ti se javili kvalificirani
            kandidati.
          </h5>
          <button class="btn">
            <router-link to="/oglas-ponuda">Kreni</router-link>
          </button>
        </div>
        <div class="header">
          <img src="../assets/17.jpg" alt="waiter" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { firebase } from "@/firebase";
import { getAuth, signOut } from "firebase/auth";

export default {
  name: "pocetna",
  methods: {
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
  width: 60%;
  text-align: center;
  font-family: Arial;
  font-size: 56px;
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

.container {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 60vh;
  font-family: Arial;
}

.card-container {
  display: flex;
  width: 1000px;
  height: auto;
  background: #fff;
  box-shadow: 1px 1px 4px 2px rgba(0, 0, 0, 0.4);
}

.header img {
  transition: 0.2s;
  width: 500px;
  height: 400px;
  object-fit: cover;
  position: relative;
  display: block;
}

.description {
  background: #fff;
  padding: 25px;
  text-align: left;
  position: relative;
}

.description h1 {
  margin: 20px 30px;
  font-size: 42px;
}

.description h5 {
  margin: 20px 30px;
  font-size: 20px;
}

.btn a {
  display: inline-block;
  padding: 10px 20px;
  background-color: #fea501;
  font-size: 17px;
  border: none;
  border-radius: 5px;
  color: #000;
  cursor: pointer;
  margin: 30px;
  position: absolute;
  bottom: 0px;
  font-family: Arial;
  text-decoration: none;
}

.btn a:hover {
  background-color: #fff;
  color: #000;
  border: 1px;
  border-style: solid;
  border-color: #fea501;
}
</style>
