<template>
  <div class="clearfix">
    <h2 class="text">💬 คอมเมนต์</h2>
    <b-form-textarea
      class="mb-2"
      v-model="text"
      placeholder="Enter something..."
      rows="3"
      max-rows="6"
    ></b-form-textarea>
    <b-button :disabled="disabled" class="float-right" @click="post" variant="outline-primary text">โพสต์</b-button>
    <b-modal
      v-model="showAlert"
      title="ผิดพลาด"
      ok-only
      ok-title="ตกลง"
      headerBgVariant="danger"
      headerTextVariant="light"
      hide-footer>
      <Alert msg="กรุณาเข้าสู่ระบบก่อนค่ะ"/>
    </b-modal>
  </div>
</template>

<script>
import axios from 'axios'
import Alert from '@/components/Modal/ModalAlert'

export default {
  components: {
    Alert
  },
  props: {
    list: {
      type: [Array, Object],
      default: () => ({})
    },
    isFetch: {
      type: Boolean,
      default: false
    },
    id: {
      type: String,
      required: true
    }
  },
  mounted () {
    if (this.isAuth) {
      this.disabled = true
    }
  },
  computed: {
    isAuth () {
      return this.$store.state.isAuth
    }
  },
  watch: {
    text (val) {
      if (this.valid(val)) {
        this.disabled = false
      } else {
        this.disabled = true
      }
    },
    isAuth (v) {
      if (v) this.disabled = true
    }
  },
  data: () => ({
    text: '',
    disabled: false,
    showAlert: false
  }),
  methods: {
    async post () {
      if (this.isAuth) {
        const cloneObj = {
          ...this.list,
          comments: [ ...this.list.comments, { name: this.$store.state.name, msg: this.text } ] }
        await axios.patch(`https://ez-json-server.herokuapp.com/reviews/${this.id}`, cloneObj)
        this.$emit('post', !this.isFetch)
        this.text = ''
      } else {
        this.showAlert = true
      }
    },
    valid (val) {
      const pattern = '[a-zA-Z0-9_]+.*$'
      const isValid = new RegExp(pattern)

      return isValid.test(val)
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Kanit');

.text {
  font-family: 'Kanit', sans-serif;

}
</style>
