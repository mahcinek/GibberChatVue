<template>
  <q-layout view="lHh Lpr lFf">
    <q-layout-header>
      <q-toolbar
        color="primary"
        :glossy="$q.theme === 'mat'"
        :inverted="$q.theme === 'ios'"
      >
        <q-btn
          flat
          dense
          round
          @click="leftDrawerOpen = !leftDrawerOpen"
          aria-label="Menu"
        >
          <q-icon name="menu" />
        </q-btn>

        <q-toolbar-title>
          GibberChat
          <div slot="subtitle">Running on Quasar v{{ $q.version }} VUE.JS Framework</div>
        </q-toolbar-title>
      </q-toolbar>
    </q-layout-header>

    <q-layout-drawer
      v-model="leftDrawerOpen"
      :content-class="$q.theme === 'mat' ? 'bg-grey-2' : null"
    >
      <q-list
        no-border
        link
        inset-delimiter
      >
        <q-list-header>Rooms</q-list-header>
            <q-item v-if=userInfoComplete  v-for="(msg, index) in rooms"
              :key="`avatar-${index}`"
              @click.native="directToChat(msg.room_id)">
              <q-item-main :label="msg.room_title" ></q-item-main>
          </q-item>
      </q-list>
    </q-layout-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import { openURL } from 'quasar'

export default {
  name: 'MyLayout',
  data () {
    return {
      leftDrawerOpen: true,
      token: 'TOKeN',
      userInfo: '',
      userInfoComplete: false,
      rooms: [],
      goodToken: 'oPa9kv7iYv5vkmqeoxPBrUWgb1FRDejlWmkZB1srh7KPWY3xnT'
    }
  },
  methods: {
    openURL,
    directToChat (id) {
      this.$router.push({ path: `/chat/${id}` })
    },
    say: function (message) {
      alert(message)
    }
  },
  created () {
    let instance = this
    this.$q.dialog({
      itle: 'Prompt',
      message: 'Podaj user token',
      prompt: {
        model: '',
        type: 'text' // optional
      },
      color: 'secondary'
    }).then(data => {
      this.token = data
      this.$axios.get('http://localhost:4000/api/user', {
        params: {
          access_token: this.token,
          auth_token: this.goodToken
        }}).then(function (response) {
        // handle success
        console.log(response.data)
        instance.userInfo = response.data
        instance.$store.state.example.userInfo = response.data
        instance.rooms = response.data.tokens
        instance.userInfoComplete = true
      }).catch(function (error) {
        // handle error
        console.log(error)
        instance.$q.dialog({
          title: 'Błąd',
          message: 'Zły token/brak połączenia z serwerem',
          color: 'secondary'
        })
      })
    })
  }
}
</script>

<style>
</style>
