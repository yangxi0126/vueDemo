<template>
  <div @click.stop.prevent="play({option:{uid:Data.song.uid,sheetCode:sheetCode,from:from}})" class="single-list-box">
    <div class="single-list-text" :class="Data.song.uid==currentPlayUid?'playing':''">
      <span class="single-list-text-title">{{Data.song.title}}</span>
      <span v-if="Data.song.markTitle.quality==2" class="single-list-text-quality icon sq">&#xe8a9;</span>
      <span v-if="Data.song.markTitle.quality==1" class="single-list-text-quality icon hq">&#xe8ab;</span>
      <span v-if="Data.song.markTitle.mv" class="single-list-text-quality icon mv">&#xe6af;</span>
      <span v-if="Data.song.markTitle.exclusive" class="text icon exclusive">&#xe6b0;</span>
      <span class="single-list-text-artist">-&nbsp;{{Data.song.artist}}</span>
    </div>
    <div class="single-list-isPlay" v-if="Data.song.uid==currentPlayUid">
      <play-animation :isPlay="playStatus"></play-animation>
    </div>
    <div class="single-list-control" v-if="from=='list'">
      <div class="single-list-isLike" @click.stop.prevent="isLike({uid:Data.song.uid})">
        <span class="icon" :class="isLikeFilter(Data.song.uid)==true?'like':''" v-html="isLikeFilter(Data.song.uid)==true?'&#xe99f;':'&#xe613;'"></span>
      </div>
      <div class="single-list-delete" @click.stop.prevent="deleteSingleInPlayList({uid:Data.song.uid})">
        <span class="icon">&#xe6bf;</span>
      </div>
    </div>
  </div>
</template>
<script>
  import {mapGetters, mapActions} from 'vuex'
  import playAnimation from './playingAnimation'
  export default{
    name: 'singleList',
    props: {
      from: {
        required: true
      },
      Data: {
        type: Object,
        required: true
      },
      sheetCode: {
        required: true
      },
      playStatus: {
        required: true
      },
      currentPlayUid: {
        required: true
      }
    },
    components: {
      playAnimation
    },
    computed: {
      ...mapGetters({
        userData: 'userData'
      })
    },
    methods: {
      ...mapActions({
        isLike: 'updateFilter',
        deleteSingleInPlayList: 'deleteSingleInPlayList',
        play: 'playerSet'
      }),
      isLikeFilter (uid) {
        return !!this.userData.basicInfo.favourite.uidFilter[uid]
      }
    }
  }
</script>
