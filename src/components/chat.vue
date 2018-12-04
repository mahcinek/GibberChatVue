<template>
<div style="width: 500px; max-width: 90vw;">
      <q-input v-model="m_body" @keyup.enter.native="sendM"/>
      <q-btn icon="create" label="Nowa wiadomość" @click="sendM"  class="float-right"/>
      <q-chat-message
        v-for="(msg, index) in messages"
        :key="`message-${index}`"
        :name="msg.nick"
        :text="[msg.body]"
      />
      <!-- <q-chat-message
        name="Vladimir"
      >
        <q-spinner-dots size="2rem" />
      </q-chat-message> -->
    </div>
</template>
<script>
import { Socket } from 'phoenix-socket'
export default {
  props: ['id'],
  methods: {
    setRoomInfo () {
      let usInfo = this.userInfo
      let inst = this
      usInfo.tokens.forEach(function (value) { if (value.room_id === inst.roomId) { inst.roomInfo = value } })
    },
    sendM () {
      this.channel.push('message:new', { body: this.m_body, options: '' })
      this.dTime = new Date().getTime()
      this.m_body = ''
    },
    changeM () {
      return []
    },
    handleM (m) {
      console.log(m)
      return { nick: m.nick, body: m.body, options: m.opts, inserted_at: m.timestamp }
    },
    downloadMessages () {
      let instance = this
      this.$axios.get('http://localhost:4000/api/room_messages', {
        params: {
          room_token: this.roomInfo.room_access_token,
          user_token: this.userInfo.access_token
        }}).then(function (response) {
        // handle success
        // console.log(response.data)
        instance.messages = response.data.messages
      }).catch(function (error) {
        // handle error
        console.log(error)
        instance.$q.dialog({
          title: 'Błąd',
          message: 'brak połączenia z serwerem',
          color: 'secondary'
        })
      })
    }
  },
  computed: {
    userInfoGetter: function () {
      return this.$store.state.example.userInfo
    }},
  created () {
    this.roomId = parseInt(this.$route.params.id)
    this.userInfo = this.userInfoGetter
    this.setRoomInfo()
    this.downloadMessages()
    this.a = new Socket('ws://0.0.0.0:4000/chat', {params: {access_token: this.userInfo.access_token}})
    this.a.connect()
    this.channel = this.a.channel('chatroom:' + this.roomInfo.room_access_token, {})
    this.channel.on('message:new', payload => {
      payload.received_at = Date()
      // console.log(payload)
      console.log(new Date().getTime() - this.dTime)
      this.messages.unshift(this.handleM(payload))
    })
    this.channel.join()
      .receive('ok', response => { console.log('Joined successfully', response) })
      .receive('error', response => { console.log('Unable to join', response) })
  },
  data () {
    return {
      message: '',
      roomId: 0,
      body: '',
      m_body: '',
      userInfo: '',
      roomInfo: '',
      channel: '',
      dTime: 0,
      a: [],
      m: [],
      messages: [
        {
          label: 'Friday, 18th'
        },
        {
          name: 'Vladimir',
          text: ['How are you?'],
          avatar: 'statics/boy-avatar.png',
          stamp: 'Yesterday 13:34'
        },
        {
          name: 'Jane',
          text: ['I\'m good, thank you!', 'And you?'],
          sent: true,
          textColor: 'white',
          bgColor: 'black',
          avatar: 'statics/linux-avatar.png',
          stamp: 'Yesterday at 13:50'
        },
        {
          name: 'Jane',
          text: ['And you?'],
          sent: true,
          avatar: 'statics/linux-avatar.png',
          stamp: 'Yesterday at 13:51'
        },
        {
          label: 'Saturday, 19th'
        },
        {
          name: 'Vladimir',
          bgColor: 'amber',
          textColor: 'white',
          text: ['Fine. Nice weather today, right?', 'Hmm...'],
          avatar: 'statics/boy-avatar.png',
          stamp: '13:55'
        },

        {
          label: 'Sunday, 20th'
        },
        {
          name: 'Vladimir',
          text: ['How are you?'],
          avatar: 'statics/boy-avatar.png',
          stamp: 'Yesterday 13:34'
        },
        {
          name: 'Jane',
          text: ['I\'m good, thank you!', 'And you?'],
          sent: true,
          avatar: 'statics/linux-avatar.png',
          stamp: 'Yesterday at 13:50'
        },
        {
          name: 'Jane',
          text: ['And you?'],
          sent: true,
          avatar: 'statics/linux-avatar.png',
          stamp: 'Yesterday at 13:51'
        },
        {
          label: 'Monday, 20th'
        },
        {
          name: 'Vladimir',
          text: ['Fine. Nice weather today, right?', 'Hmm...'],
          avatar: 'statics/boy-avatar.png',
          stamp: '13:55'
        },

        {
          label: 'Tuesday, 21th'
        },
        {
          name: 'Vladimir',
          text: ['How are you?'],
          avatar: 'statics/boy-avatar.png',
          stamp: 'Yesterday 13:34'
        },
        {
          name: 'Jane',
          text: ['I\'m good, thank you!', 'And you?'],
          sent: true,
          avatar: 'statics/linux-avatar.png',
          stamp: 'Yesterday at 13:50'
        },
        {
          name: 'Jane',
          text: ['And you?'],
          sent: true,
          avatar: 'statics/linux-avatar.png',
          stamp: 'Yesterday at 13:51'
        },
        {
          label: 'Sunday, 19th'
        },
        {
          name: 'Vladimir',
          text: ['Fine. Nice weather today, right?', 'Hmm...'],
          avatar: 'statics/boy-avatar.png',
          stamp: '13:55'
        },

        {
          label: 'Wednesday, 22th'
        },
        {
          name: 'Vladimir',
          text: ['How are you?'],
          avatar: 'statics/boy-avatar.png',
          stamp: 'Yesterday 13:34'
        },
        {
          name: 'Jane',
          text: ['I\'m good, thank you!', 'And you?'],
          sent: true,
          avatar: 'statics/linux-avatar.png',
          stamp: 'Yesterday at 13:50'
        },
        {
          name: 'Jane',
          text: ['And you?'],
          sent: true,
          avatar: 'statics/linux-avatar.png',
          stamp: 'Yesterday at 13:51'
        },
        {
          label: 'Thursday, 23th'
        },
        {
          name: 'Vladimir',
          text: ['Fine. Nice weather today, right?', 'Hmm...'],
          avatar: 'statics/boy-avatar.png',
          stamp: '13:55'
        }
      ]
    }
  }
}
</script>
