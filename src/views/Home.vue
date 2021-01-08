<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title>Peliculas</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-item v-if="currentUser">
        <ion-label>Pulsa para realizar acciones</ion-label>
      </ion-item>
      <ion-list v-for="film in films" :key="film.id">
        <!--<ion-item style="--border-color:grey">-->
        <ion-item v-if="!currentUser" class="ion-item">
          <ion-label>
            <h2>{{film.title}}</h2>
            <p>{{film.description}}</p>
            </ion-label>
            <div>
              <span style="vertical-align: middle">{{film.calification}}</span>&nbsp;
              <ion-icon style="vertical-align:middle" :icon="star"></ion-icon>
            </div>
        </ion-item>
        <ion-item v-if="currentUser" @click="presentActionSheet(film.id)" class="ion-item">
          <ion-label>
            <h2>{{film.title}}</h2>
            <p>{{film.description}}</p>
            </ion-label>
            <div>
              <span style="vertical-align: middle">{{film.calification}}</span>&nbsp;
              <ion-icon style="vertical-align:middle" :icon="star"></ion-icon>
            </div>
        </ion-item>
      </ion-list>
    </ion-content>
  </ion-page>
  
</template>

<script>
import { /*IonContent,*/IonIcon, IonHeader, IonPage, IonTitle, IonToolbar, actionSheetController } from '@ionic/vue';
import { defineComponent } from 'vue';
import { star } from 'ionicons/icons';

  class Film{
        constructor(title, year, description, genre, calification, duration, link){
            this.title = title;
            this.year = year;
            this.description = description;
            this.genre = genre;
            this.calification = calification;
            this.duration = duration;
            this.link = link;
        }
    }

export default defineComponent({
  name: 'Home',
  components: {
    //IonContent,
    IonIcon,
    IonHeader,
    IonPage,
    IonTitle,
    IonToolbar
  },
  computed: {
      currentUser() {
          //return this.$store.state.auth.user;/
          return JSON.parse(localStorage.getItem('user'))
      },
  },
  data() {
      return {
        film: new Film(),
        films: [],
        star,
        //api : new Servicio_API('http://localhost:3000')
      }
    },
    created(){
        this.obtenerFilms()
    },
    updated: function () {
    this.$nextTick(function () {
      this.obtenerFilms})
    },
    methods: {
      async presentActionSheet(id) {
      const actionSheet = await actionSheetController
        .create({
          header: 'Acciones',
          cssClass: 'my-custom-class',
          buttons: [
            {
              text: 'Borrar',
              role: 'destructive',
              handler: () => {
                this.deleteFilm(id)
                console.log('Delete clicked')
              },
            },
          ],
        });
      return actionSheet.present();
    },
        obtenerFilms(){
            fetch('http://192.168.1.10:3000/films')
            .then(res => res.json())
            .then(data => {
                console.log("Listado de peliculas")
                this.films = data.film
                console.log(this.films)
            })
        },
        deleteFilm(id){
            console.log("Borrar una pelicula")
            fetch('http://localhost:3000/films/' + id,{
                method: 'DELETE',
                headers:{
                    'Accept': 'application/json',
                    'Content-type': 'application/json',
                    'Authorization': 'Bearer ' + this.currentUser.token
                }
            })
            .then(res => res.json())
            .then(data => {
                console.log(data)
                this.obtenerFilms()
            })
            console.log(id)
        }
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

.ion-item { 
  --border-color:rgb(58, 54, 54);
}

</style>