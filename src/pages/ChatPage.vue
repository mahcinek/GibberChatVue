<template>
  <q-page padding class="row justify-center">
      <chat id:idChat></chat>
  </q-page>
</template>

<script>
import chat from '../components/chat'
export default {
  name: 'Chat',
  userInfo: [],
  data () {
    return {
      pageTitle: 'Chat',
      idChat: 0
    }
  },
  components: {
    chat
  },
  created () {
    this.idChat = this.$route.params.id
  }
}

</script>
