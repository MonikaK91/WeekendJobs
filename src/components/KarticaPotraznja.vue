<template>
  <div class="container">
    <div class="card-container">
      <div class="header">
        <img :src="info.image" />
        <h2>{{ info.firstName }} {{ info.lastName }}</h2>
        <h4>{{ info.job }}</h4>
      </div>
      <div class="description">
        <p>
          {{ info.message }}
        </p>
        <p>
          Email:<a href="mailto:info.email">{{ info.email }}</a> <br />
          Telefon: {{ info.phone }} <br />
          Mjesto stanovanja: {{ info.city }}
          <a
            href="#"
            v-if="info.e_mail == this.currentUser"
            @click="deleteAd(info.id)"
            ><ion-icon name="trash"></ion-icon
          ></a>
          <!-- ako je email autora oglasa isti kao i email trenutnog korisnika 
          onda se prikazuje ikona i može se obrisati oglas pomoću id-a tog oglasa -->
        </p>
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
  name: "KarticaPotraznja",
  data() {
    return {
      currentUser: store.currentUser,
    };
  },
  methods: {
    /* metoda za brisanje oglasa odnosno dokumenta sa firestore-a 
    koja kao id uzima id koji je predan gore u pozivu metode */
    async deleteAd(id) {
      const c = confirm("Jeste li sigurni da želite obrisati oglas? ");
      if (c) {
        await deleteDoc(doc(db, "oglasiPotraznja", id));
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
  height: 45vh;
  font-family: Arial;
}

.card-container {
  display: flex;
  width: 800px;
  height: auto;
  background: #fff;
  border-radius: 2px;
  box-shadow: 1px 1px 4px 2px rgba(0, 0, 0, 0.2);
}

.header {
  border-right: 1px solid #ddd;
  padding: 20px;
  text-align: center;
  width: 250px;
}

.header img {
  border: 2px solid grey;
  margin: 10px 35px;
  transition: 0.2s;
  width: 150px;
  height: 130px;
  object-fit: cover;
  position: relative;
  display: block;
  border-radius: 20px;
}

.header h2 {
  color: #fea501;
  margin: 10px 0px;
}

.header h4 {
  margin: 10px 0px;
}

.description {
  background: #fbfbfb;
  padding: 20px;
  text-align: center;
  position: relative;
  width: 550px;
}

.description p {
  margin: 20px 30px;
}

.description a {
  display: inline-block;
  padding: 0 15px;
  color: #fea501;
}

.description ion-icon {
  position: absolute;
  bottom: 0;
  right: 0;
  color: #fea501;
  height: 50px;
  width: 32px;
}

.description ion-icon:hover {
  color: #000;
}
</style>
