<template>
 <div class="m-row">
 <div class="title"><span>斗鱼TV</span> <strong>{{getGameTitle}}</strong></div>
    <div class="live-list">
      <rooms-item v-for="roomlist in roomlists" :room="roomlist"></rooms-item>
    </div>
    <more-button>
      <div class="more-button" v-if="!hidden">
        <div v-show="!loading" @click="loadMore">加载更多</div>
        <div v-show="loading" class="ball-pulse"><div></div><div></div><div></div></div>
      </div>
    </more-button>
 </div>
</template>
<script>
  import RoomsItem from './rooms-item'
  import MoreButton from '../../components/more-button.vue'
  import {mapGetters} from 'vuex'

  export default {
    data () {
      return {
        obj: {id: this.$route.params.id, limit: 8},
        hidden: false,
        loading: false
      }
    },
    computed: {
      ...mapGetters({
        roomlists: 'getRoomlists',
        getGameTitle: 'getGameTitle'
      })
    },
    mounted () {
      this.$store.dispatch('fetchRoomLists', this.obj)
      this.$store.dispatch('getGameTitle', this.$route.params.name)
    },
    methods: {
      loadMore () {
        this.obj.limit = this.obj.limit + 8
        if (this.obj.limit > 100) {
          this.obj.limit = 100
          this.hidden = true
        }
        setTimeout(() => {
          this.$store.dispatch('fetchRoomLists', this.obj)
          this.loading = true
          setTimeout(() => { this.loading = false }, 1000)
        }, 1500)
      }
    },
    components: {
      RoomsItem, MoreButton
    }
  }
</script>
<style lang='less'>
  .m-row {
    .title {
      font-size: 14px;
      margin-left: 10px;
      line-height: 1.13333333rem;
      strong {
        margin-left: .39525rem;
        color: #FA7122;
      }
    }
  }
</style>
