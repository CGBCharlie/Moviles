<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Tab 3</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Tab 3</ion-title>
        </ion-toolbar>
      </ion-header>
      <ion-item>
        <ion-label>Número de claves nuevas</ion-label>
        <ion-input
          Type="number"
          min="1"
          :value="claves"
          @ionInput="claves = parseInt($event.target.value)"
        ></ion-input>
      </ion-item>
      <ion-button expand="block" @click="agregarClaves()"
        >Agregar claves</ion-button
      >
      <alert-controller></alert-controller>
      <ExploreContainer name="Tab 3 page" />
    </ion-content>
  </ion-page>
</template>

<script>
import {
  IonPage,
  IonHeader,
  IonToolbar,
  IonTitle,
  IonContent,
  alertController,
  IonButton,
  IonItem,
  IonInput,
  IonLabel,
} from "@ionic/vue";
import { getDatabase, ref, push } from "firebase/database";

export default {
  name: "Tab3Page",
  components: {
    IonHeader,
    IonToolbar,
    IonTitle,
    IonContent,
    IonPage,
    alertController,
    IonButton,
    IonItem,
    IonInput,
    IonLabel,
  },
  data() {
    return {
      claves: 1,
    };
  },
  methods: {
    async agregarClaves() {
      const db = getDatabase();
      var i;
      var errores = 0;
      for (i = 0; i < this.claves; i++) {
        push(ref(db, "claves/"), {
          status: "",
          usuario: "",
        })
          .then(async () => {
            // Data saved successfully!
          })
          .catch(async (error) => {
            console.log(error);
            errores++;
          });
      }
      if (errores > 0) {
        const alert = await alertController.create({
          cssClass: "clase claves no agregadas",
          header: "Claves NO agregadas",
          subHeader: "Error",
          message: "No se agregagor las claves",
          buttons: ["Aceptar"],
        });
        await alert.present();
        const { role } = await alert.onDidDismiss();
        console.log("onDidDismiss resolved with role", role);
      } else {
        const alert = await alertController.create({
          cssClass: "clase claves agregadas",
          header: "Claves agregadas",
          subHeader: "Éxito",
          message: "Se agregaron las claves con éxito",
          buttons: ["Aceptar"],
        });
        await alert.present();
        const { role } = await alert.onDidDismiss();
        console.log("onDidDismiss resolved with role", role);
      }
    },
  },
};
</script>
