<template>
  <amplify-authenticator>
  <v-app id="inspire">
    <v-navigation-drawer
      v-model="drawer"
      app
    >
     <v-sheet
        color="grey lighten-4"
        class="pa-4"
      >
        <v-avatar
          class="mb-4"
          color="grey darken-1"
          size="64"
        ></v-avatar>
        <div v-if="authState === 'signedin' && user">
        <div>{{user.username}}</div>
        </div>
      </v-sheet>
      <amplify-sign-out></amplify-sign-out>
    </v-navigation-drawer>

    <v-app-bar app>
      <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>

      <v-toolbar-title>Your Application Name</v-toolbar-title>

    </v-app-bar>

    <v-main>
      <Home />
    </v-main>
  </v-app>
  </amplify-authenticator>
</template>

<script>
import Home from './components/Home';
import { onAuthUIStateChange } from '@aws-amplify/ui-components'

export default {
  name: 'App',
  created() {
    onAuthUIStateChange((authState, authData) => {
      this.authState = authState;
      this.user = authData;
    })
  },

  components: {
    Home,
  },
  data() {
    return {
        drawer: null,
        user: undefined,
        authState: undefined,
        formFields: [
            { type: "username" },
            { type: "password" },
            { type: "email" }
        ]
    }
  },
  beforeDestroy() {
    return onAuthUIStateChange;
  }
};
</script>