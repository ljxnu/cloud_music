<template>
  <div class="user-playlist">
    <l-area class="area" title="创建的歌单">
      <normal-songs-matrices :col="5" :datas="selfPlaylist"/>
    </l-area>

    <l-area class="area" title="收藏的歌单">
      <normal-songs-matrices :col="5" :datas="collPlaylist"/>
    </l-area>
  </div>
</template>

<script>
  import {playlist} from "@/network/request_uesr";
  import NormalSongsMatrices from "@/components/content/matrices/NormalSongsMatrices";
  import LArea from "@/components/common/LArea";

  export default {
    name: "UserPlaylist",
    components: {LArea, NormalSongsMatrices},
    props: {
      uid: { type: [String, Number] }
    },

    data() {
      return {
        selfPlaylist:[],
        collPlaylist:[],
      }
    },

    created() {
      this.refresh()
    },

    methods: {
      refresh() {
        playlist(this.uid).then(result => {
          this.selfPlaylist = result['playlist'].filter((value) => {
            let id = value['creator']['userId']
            return parseInt(id) === parseInt(this.uid)
          })
          this.collPlaylist = result['playlist'].filter((value) => {
            let id = value['creator']['userId']
            return parseInt(id) !== parseInt(this.uid)
          })
        })
      }
    },

    watch: {
      uid() {
        this.refresh()
      }
    }
  }
</script>

<style scoped>
  .area {
    margin-top: 20px;
  }
</style>
