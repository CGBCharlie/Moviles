<template>
  <ion-app>
    <ion-router-outlet />
  </ion-app>
</template>

<script>
import { IonApp, IonRouterOutlet } from "@ionic/vue";
import { defineComponent } from "vue";
import { getAuth, signInWithEmailAndPassword } from "firebase/auth";

export default defineComponent({
  name: "App",
  components: {
    IonApp,
    IonRouterOutlet,
  },
  created() {
    const auth = getAuth();
    signInWithEmailAndPassword(auth, "carlosgarcia@gmail.com", "carlos1")
      .then((userCredential) => {
        // Signed in
        const user = userCredential.user;
        if (user == null) {
          console.log("no autenticado");
        } else {
          console.log("usuario autenticado", user);
        }
      })
      .catch((error) => {
        const errorCode = error.code;
        const errorMessage = error.message;
        console.log(errorCode, errorMessage);
      });
  },
});
</script>