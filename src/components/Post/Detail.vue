<template>
  <div>
    <div v-html="review"/>
    <social-sharing :url="url" inline-template>
      <div class="d-flex justify-content-center">
        <network network="facebook" class="m-2">
         <i class="fab fa-facebook img" id="facebook"/>
        </network>
        <network network="twitter" class="m-2">
          <i class="fab fa-twitter-square img" id="twitter"/>
        </network>
      </div>
    </social-sharing>
  </div>
</template>

<script>
export default {
  props: {
    review: {
      type: String,
      default: ''
    }
  },
  mounted () {
    this.url = window.location.href
  },
  data: () => ({
    url: null
  })
}
</script>

<style>
.img {
  font-size: 2.8em;
}
#twitter {
  color: #3498db;
}
#facebook {
  color: #3C5A99
}
@media (max-width: 768px) {
  .img {
    font-size: 2.2em;
  }
}
</style>
