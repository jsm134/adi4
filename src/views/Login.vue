<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title>Login</ion-title>
      </ion-toolbar>
    </ion-header>    
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
      </ion-header>
       <ion-card-header>
      <ion-card-subtitle>Aquí usted podrá acceder a su cuenta</ion-card-subtitle>
    </ion-card-header>
      <ion-card>
        <form @submit.prevent="login">
        <ion-item>
          <ion-label position="floating">Username</ion-label>
          <ion-input v-model="user.user" id="username" required></ion-input>
        </ion-item>
        <ion-item>
          <ion-label position="floating">Password</ion-label>
          <ion-input type="password" v-model="user.pass" id="password" required></ion-input>
        </ion-item>

          <ion-button type="submit" expand="block">
            Sign In
            <ion-icon slot="end" :icon="logIn"></ion-icon>
          </ion-button>

        </form>
      </ion-card>
    </ion-content>
    
  </ion-page>
</template>

<script>
import { IonContent, IonHeader, IonPage, IonTitle, IonToolbar, IonLabel, IonButton, IonInput } from '@ionic/vue';
import { defineComponent } from 'vue';

class User{
    constructor(username, password){
        this.user = username;
        this.pass = password;
    }
}

export default defineComponent({
  name: 'Login',
  data() {
    return {
      uname: '',
      upass: '',
      user: new User(),
      loading: false,
      message: '',
      error: null
    };
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
  },
    methods: {
      login(){
        //user.username = this.
        //console.log(this.user)
        console.log(this.user)
        fetch('http://localhost:3000/login' ,{
                method: 'POST',
                body: JSON.stringify(this.user),
                headers:{
                    'Accept': 'application/json',
                    'Content-type': 'application/json',
                    //'Authorization': 'Bearer ' + this.currentUser.token
                }
            })
            .then(res => res.json())
            .then(data => {
                console.log("Login")
                console.log(data)
                if (data.mensaje == "OK") {
                  data.user = this.user.user
                  localStorage.setItem('user', JSON.stringify(data))
                  /*location.reload().then(
                    this.$router.push('/')
                  )*/
                  window.location.href = 'http://localhost:8080/';
                }
            })
      }
    /*handleLogin() {
      this.loading = true;
      if (this.user.username && this.user.password) {
        this.$store.dispatch('auth/login', this.user).then(
          () => {
            console.log("usuario actual")
            if(!this.loggedIn){
              this.error="Usuario o contraseña incorrectos"
              console.log(this.error)
            }else{
              //function getHeaders() {
                return {
                  'Authorization': 'Bearer: ' + 
                  window.localStorage.getItem('access_token')
                }  
              //}
              //locate.reload().then(()=> this.$router.push('/films'))
              //this.$router.push('/');
              //next('/films')
            }
          },
        );
      }
    }*/
  }
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