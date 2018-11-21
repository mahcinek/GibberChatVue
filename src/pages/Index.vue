<template>
  <q-page padding class="row justify-center">
    <div style="width: 500px; max-width: 90vw;">
      <q-chat-message
        v-for="(msg, index) in messages"
        :key="`avatar-${index}`"
        :label="msg.label"
        :sent="msg.sent"
        :text-color="msg.textColor"
        :bg-color="msg.bgColor"
        :name="msg.name"
        :text="msg.text"
        :stamp="msg.stamp"
      />
      <q-chat-message
        name="Vladimir"
      >
        <q-spinner-dots size="2rem" />
      </q-chat-message>
    </div>
  </q-page>
</template>

<script>
import { Socket } from 'phoenix-socket'
export default {
  created () {
    this.a = new Socket('ws://0.0.0.0:4000/chat', {params: {access_token: 'abcdf'}})
    this.a.connect()
  },
  data () {
    return {
      message: '',
      a: [],
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
