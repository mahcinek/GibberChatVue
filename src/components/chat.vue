<template>
<div style="width: 80%; max-width: 90vw;">
  <div >
      <q-input v-model="m_body" inverted color="secondary" helper="Write a message" @keyup.enter.native="sendM" :stack-label="roomInfo.room_title"/>
      <q-btn icon="create" label="New message" @click="sendM"  class="float-right"/>
  </div>
<div style="margin: 40px">
      <q-chat-message
        v-for="(msg, index) in messages"
        :key="`message-${index}`"
        :name="msg.nick"
        :text="[msg.body]"
        :bg-color="msg.options.col"
        :sent="msg.nick == userInfo.nick"
      />
</div>

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
  watch: {
    // call again the method if the route changes
    '$route': 'downloadDataAndConnect'
  },
  methods: {
    setRoomInfo () {
      let usInfo = this.userInfo
      let inst = this
      usInfo.tokens.forEach(function (value) { if (value.room_id === inst.roomId) { inst.roomInfo = value } })
    },
    sendM () {
      this.channel.push('message:new', { body: this.m_body, options: JSON.stringify(this.userInfo.options) })
      this.dTime = new Date().getTime()
      this.m_body = ''
    },
    changeM () {
      return []
    },
    handleM (m) {
      console.log(m)
      console.log('handling')
      return { nick: m.nick, body: m.body, options: JSON.parse(m.options), inserted_at: m.timestamp }
    },
    handleM2 (m) {
      console.log(m)
      console.log('handling')
      return { nick: m.nick, body: m.body, options: JSON.parse(m.options), inserted_at: m.timestamp }
    },
    downloadMessages () {
      let instance = this
      this.$axios.get('http://localhost:4000/api/room_messages', {
        params: {
          room_token: this.roomInfo.room_access_token,
          user_token: this.userInfo.access_token
        }}).then(function (response) {
        // handle success
        console.log(response.data.messages)
        let temp = []
        response.data.messages.forEach(function (value) { temp.push(instance.handleM2(value)) })
        console.log('temp')
        console.log(temp)
        instance.messages = temp
      }).catch(function (error) {
        // handle error
        console.log(error)
        instance.$q.dialog({
          title: 'Błąd',
          message: 'brak połączenia z serwerem',
          color: 'secondary'
        })
      })
    },
    downloadDataAndConnect () {
      this.roomId = parseInt(this.$route.params.id)
      this.userInfo = this.userInfoGetter
      if (this.a !== '') {
        console.log('disconnectiong')
        console.log(this.a)
        this.a.disconnect()
        this.a = []
      }
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
    }
  },
  computed: {
    userInfoGetter: function () {
      return this.$store.state.example.userInfo
    }},
  created () {
    this.downloadDataAndConnect()
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
      a: '',
      m: [],
      messages: [
        {
          nick: 'Vladimir',
          body: ['How are you?'],
          inserted_at: 'Yesterday 13:34',
          options: {col: 'red'}
        }
      ]
    }
  }
}
</script>
