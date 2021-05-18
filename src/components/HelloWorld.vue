<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p>
      For a guide and recipes on how to configure / customize this project,<br>
      check out the
      <a href="https://cli.vuejs.org" target="_blank" rel="noopener">vue-cli documentation</a>.
    </p>
    <h3>Installed CLI Plugins</h3>
    <ul>
      <li><a href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-babel" target="_blank" rel="noopener">babel</a></li>
      <li><a href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-pwa" target="_blank" rel="noopener">pwa</a></li>
      <li><a href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-router" target="_blank" rel="noopener">router</a></li>
      <li><a href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-vuex" target="_blank" rel="noopener">vuex</a></li>
    </ul>
    <h3>Essential Links</h3>
    <ul>
      <li><a href="https://vuejs.org" target="_blank" rel="noopener">Core Docs</a></li>
      <li><a href="https://forum.vuejs.org" target="_blank" rel="noopener">Forum</a></li>
      <li><a href="https://chat.vuejs.org" target="_blank" rel="noopener">Community Chat</a></li>
      <li><a href="https://twitter.com/vuejs" target="_blank" rel="noopener">Twitter</a></li>
      <li><a href="https://news.vuejs.org" target="_blank" rel="noopener">News</a></li>
    </ul>
    <h3>Ecosystem</h3>
    <ul>
      <li><a href="https://router.vuejs.org" target="_blank" rel="noopener">vue-router</a></li>
      <li><a href="https://vuex.vuejs.org" target="_blank" rel="noopener">vuex</a></li>
      <li><a href="https://github.com/vuejs/vue-devtools#vue-devtools" target="_blank" rel="noopener">vue-devtools</a></li>
      <li><a href="https://vue-loader.vuejs.org" target="_blank" rel="noopener">vue-loader</a></li>
      <li><a href="https://github.com/vuejs/awesome-vue" target="_blank" rel="noopener">awesome-vue</a></li>
    </ul>
  </div>
</template>

<script>
//uso de firebase
import firebase from 'firebase/app'
import 'firebase/app'
import 'firebase/messaging'

//uso de axios
import axios from 'axios'


export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  methods:{
    salvarTokenNotificacion(token){
      const token_autorizacion = "2db40115a845ef599dd844135fe05efa34344085"
      const registrar_token_url = 'http://localhost:8000/registrar-notificacion-token/'
      const payload = {
        registration_id: token,
        type: 'web'
      }
      //uso de axios
      axios.post(registrar_token_url,payload,{
        headers: {
          'Authorization': `Token ${token_autorizacion}`
        },
      })
      .then((response) => {
        console.log('Token de notificacion guardado de manera satisfactoria.')
        console.log(response.data)
      })
      .catch((error)=>{
        console.log('No se pudo salval el token')
      })
    }
  },
  mounted(){
    var config = {
      apiKey: "AIzaSyCBlCCyd5wBFkLgn7E-hQfLqRLxj9NtB8o",
      authDomain: "notificaciones-1784b.firebaseapp.com",
      projectId: "notificaciones-1784b",
      storageBucket: "notificaciones-1784b.appspot.com",
      messagingSenderId: "84796054027",
      appId: "1:84796054027:web:ee8f55a92bea6c2706450e",
      measurementId: "G-0L90RNQKYY"
    }

    firebase.initializeApp(config)
    const messaging = firebase.messaging()
    messaging.usePublicVapidKey("BNB347r3tkMxI7GXmg2DKQhmvV6PaK5A90JCrddIr9SlALWcV7WLumTOCQmSMd9CfoprZpWcheES1PNkxMp5MoE")

    messaging.requestPermission()
    .then(() => {
      console.log('Notification permission granted.')
      messaging.getToken().then((token) => {
        console.log('New token created: ', token)
        this.salvarTokenNotificacion(token)
      })
    })
    .catch((err) => {
      console.log('Unable to get permission to notify.', err)
    })

  },

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
