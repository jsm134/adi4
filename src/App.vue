<template>
  <ion-page>
      <ion-tabs @ionTabsWillChange="beforeTabChange" @ionTabsDidChange="afterTabChange">
        <ion-tab-bar slot="bottom">
          <ion-tab-button tab="home" href="/home">
            <ion-icon :icon="film"></ion-icon>
            <ion-label>Peliculas</ion-label>
          </ion-tab-button>
          <ion-tab-button tab="create" href="/createFilm">
            <ion-icon :icon="addCircle"></ion-icon>
            <ion-label>Crear Pelicula</ion-label>
          </ion-tab-button>
          <ion-tab-button tab="login" href="/login">
            <ion-icon :icon="logIn"></ion-icon>
            <ion-label>Login</ion-label>
          </ion-tab-button>
          <ion-tab-button tab="logout" href="/logout">
            <ion-icon :icon="logOut"></ion-icon>
            <ion-label>Logout</ion-label>
          </ion-tab-button>
        </ion-tab-bar>
      </ion-tabs>
    </ion-page>
</template>

<script>
import { defineComponent } from 'vue';
import { 
  IonIcon, 
  IonLabel, 
  IonPage,
  IonTabBar, 
  IonTabButton, 
  IonTabs,
} from '@ionic/vue';
import { film, addCircle, logIn, logOut } from 'ionicons/icons';

export default defineComponent({
  name: 'App',
  components: { IonIcon, IonLabel, IonPage, IonTabBar, IonTabButton, IonTabs},
  setup() {
    const beforeTabChange = () => {
      // do something before tab change
    }
    const afterTabChange = () => {
      // do something after tab change
    }
    return {
      film,
      addCircle,
      logIn,
      logOut,
      beforeTabChange,
      afterTabChange
    }
  }
});
</script>