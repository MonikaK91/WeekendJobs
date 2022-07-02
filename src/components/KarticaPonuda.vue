<template>
  <div class="container">
    <div class="card-container">
      <div class="description1">
        <h2>Radno mjesto</h2>
        <div class="grid-container">
          <div class="grid-item"><h4>Naziv posla</h4></div>
          <div class="grid-item">
            <p>{{ info.job }}</p>
          </div>
          <div class="grid-item"><h4>Mjesto rada</h4></div>
          <div class="grid-item">
            <p>{{ info.workPlace }}</p>
          </div>
          <div class="grid-item"><h4>Radno vrijeme</h4></div>
          <div class="grid-item">
            <p>{{ info.workingTime }}</p>
          </div>
          <div class="grid-item"><h4>Datum isteka</h4></div>
          <div class="grid-item">
            <p>{{ info.date }}</p>
          </div>
        </div>
      </div>
      <div class="description2">
        <h2>Posloprimac</h2>
        <div class="grid-container">
          <div class="grid-item"><h4>Razina obrazovanja</h4></div>
          <div class="grid-item">
            <p>{{ info.education }}</p>
          </div>
          <div class="grid-item"><h4>Radno iskustvo</h4></div>
          <div class="grid-item">
            <p>{{ info.experience }}</p>
          </div>
        </div>
        <div class="information">
          <h4>Opis posla</h4>
          <p>
            {{ info.description }}
          </p>
          <h4>Ostale informacije</h4>
          <p>
            {{ info.information }}
          </p>
        </div>
      </div>
      <div class="description1">
        <h2>Poslodavac</h2>
        <div class="grid-container">
          <div class="grid-item"><h4>Poslodavac</h4></div>
          <div class="grid-item">
            <p>{{ info.employer }}</p>
          </div>
          <div class="grid-item"><h4>Telefon</h4></div>
          <div class="grid-item">
            <p>{{ info.phone }}</p>
          </div>
        </div>
        <a
          href="#"
          v-if="info.email == this.currentUser"
          @click="deleteAd(info.id)"
          ><ion-icon name="trash"></ion-icon
        ></a>
        <!-- ako je email autora oglasa isti kao i email trenutnog korisnika 
        onda se prikazuje ikona i može se obrisati oglas pomoću id-a tog oglasa -->
      </div>
    </div>
  </div>
</template>

<script>
import { doc, deleteDoc } from "firebase/firestore";
import { firebase, db } from "@/firebase";
import store from "@/store";

export default {
  props: ["info"],
  name: "KarticaPonuda",
  data() {
    return {
      currentUser: store.currentUser,
    };
  },
  methods: {
    /* metoda za brisanje oglasa odnosno dokumenta sa firestore-a 
    koja kao id uzima id koji je predan gore u pozivu metode */
    async deleteAd(id) {
      const c = confirm("Jeste li sigurani da želite obrisati oglas? ");
      if (c) {
        await deleteDoc(doc(db, "oglasiPonuda", id));
        console.log("Obrisano");
        alert("Uspješno obrisan oglas");
        window.location.reload();
        // nakon brisanja oglasa osvježava se stranica da se mogu vidjeti promjene
      }
    },
  },
};
</script>

<style scoped lang="scss">
.container {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 55vh;
  font-family: Arial;
}

.card-container {
  display: flex;
  width: 1200px;
  height: auto;
  background: #fff;
  border-radius: 2px;
  box-shadow: 1px 1px 4px 2px rgba(0, 0, 0, 0.4);
}

.description1 {
  width: 300px;
  position: relative;
  background: #fff;
  padding: 20px;
  text-align: left;
  border-right: 2px solid #ddd;
}

.description1 h2 {
  color: #fea501;
  margin: 10px 15px;
}

.description2 {
  width: 600px;
  position: relative;
  background: #fff;
  padding: 20px;
  text-align: left;
  border-right: 2px solid #ddd;
}

.description2 h2 {
  color: #fea501;
  margin: 10px 15px;
}

.information {
  padding: 5px 15px;
}

.information h4 {
  padding: 5px 0px;
  margin: 0px;
}

.information p {
  margin: 0px;
}

.grid-container {
  display: grid;
  grid-template-columns: 150px auto;
  background-color: #fff;
  padding: 10px;
}

.grid-item {
  background-color: #fff;
  padding: 5px;
  font-size: 16px;
  text-align: left;
}

.grid-item h4,
p {
  margin: 0px;
}

.description1 ion-icon {
  position: absolute;
  bottom: 0;
  right: 0;
  color: #fea501;
  height: 50px;
  width: 32px;
}

.description1 ion-icon:hover {
  color: #000;
}
</style>
