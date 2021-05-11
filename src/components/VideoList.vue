<template>
  <div class="col">
    <ul class="list-group">
      <!-- key를 함께 받아와야 함 -->
      <VideoListItem v-for="(video, idx) in videos" :key="idx" :video="video"
      @select-video="onSelectVideo"
      />
    </ul>
  </div>
</template>

<script>
import VideoListItem from '@/components/VideoListItem'

export default {
  name: 'VideoList',
  props: {
    videos: Array,
  },
  methods: {
    // VideoListItem에서 함께 보낸 데이터 video로 받음
    onSelectVideo: function (video) {
      // 다시 부모에게 보냄
      this.$emit('select-video', video)
    }
  },
  components: {
    VideoListItem,
  },
}
</script>

<style>
ul {
  list-style: none;
}

</style>