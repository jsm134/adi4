<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title>Crear Pelicula</ion-title>
      </ion-toolbar>
    </ion-header>
    
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
      </ion-header>
       <ion-card-header>
      <ion-card-subtitle>Aquí usted podrá crear una nueva película</ion-card-subtitle>
    </ion-card-header>
      <ion-card>
        <form @submit.prevent="addFilm">
        <ion-item>
          <ion-label position="floating">Título</ion-label>
          <ion-input v-model="film.title" id="title" required></ion-input>
        </ion-item>
        <ion-item>
          <ion-label position="floating">Descripción</ion-label>
          <ion-textarea rows="4" v-model="film.description" id="description" required></ion-textarea>
        </ion-item>
        <ion-item>
          <ion-label position="floating">Año</ion-label>
          <ion-input type="text" v-model="film.year" id="year" required></ion-input>
        </ion-item>
        <ion-item>
          <ion-label position="floating">Género</ion-label>
          <ion-input type="text" v-model="film.genre" id="genre" required></ion-input>
        </ion-item>
        <ion-item>
          <ion-label position="floating">Calificación</ion-label>
          <ion-input type="text" v-model="film.calification" id="calification" required></ion-input>
        </ion-item>
        <ion-item>
          <ion-label position="floating">Duración</ion-label>
          <ion-input type="text" v-model="film.duration" id="duration" required></ion-input>
        </ion-item>

          <ion-button type="submit" expand="block">
            Crear
            <ion-icon slot="end" :icon="logIn"></ion-icon>
          </ion-button>

        </form>
      </ion-card>
    </ion-content>
  </ion-page>
</template>

<script>
class Film{
  constructor(title, year, description, genre, calification, duration){
      this.title = title;
      this.year = year;
      this.description = description;
      this.genre = genre;
      this.calification = calification;
      this.duration = duration;
  }
}
import { IonContent, IonHeader, IonPage, IonTitle, IonToolbar, IonLabel, IonButton, IonInput, IonTextarea } from '@ionic/vue';
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'Home',
  data() {
    return {
      film: new Film(),
      films: [],
      //api : new Servicio_API('http://localhost:3000')
    }
  },
  computed: {
    currentUser() {
        //return this.$store.state.auth.user;
        return JSON.parse(localStorage.getItem('user'))
    },
  },
  methods: {
    addFilm(){
      console.log("hola")
      console.log(this.film)
      fetch('http://localhost:3000/films',{
          method: 'POST',
          body: JSON.stringify(this.film),
          headers:{
              'Accept': 'application/json',
              'Content-Type': 'application/json',
              'Authorization': 'Bearer ' + this.currentUser.token
          }
      }).then(res=>res.json())
      .then(data => {
          if(data.mensaje=="Pelicula añadida correctamente"){
            window.location.href = 'http://localhost:8080/';
          }
          //this.obtenerFilms()
          console.log(data)
      })
      this.film = new Film()

    }
  },
  components: {
    IonContent,
    IonHeader,
    IonPage,
    IonTitle,
    IonToolbar,
    IonLabel,
    IonButton,
    IonInput,
    IonTextarea,
  },
});
</script>

<style scoped>
#container {
  text-align: center;
  
  position: absolute;
  left: 0;
  right: 0;
  top: 50%;
  transform: translateY(-50%);
}

#container strong {
  font-size: 20px;
  line-height: 26px;
}

#container p {
  font-size: 16px;
  line-height: 22px;
  
  color: #8c8c8c;
  
  margin: 0;
}

#container a {
  text-decoration: none;
}
</style>