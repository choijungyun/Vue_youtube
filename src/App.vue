<template>
  <div id="app">
    <header>
      <img alt="youtube logo" src="./assets/logo.png" width="100px">
      <SearchBar @input-search="onInputSearch"/>
    </header>
    <section>
     <VideoDetail :video="selectedVideo"/>
     <!-- *<Parent :전달이름="전달값" />*
          child가 보낸 이벤트 child-input를 parent(부모)가 듣는다. @를 씀  -->
     <VideoList :videos="videos" @select-video="onVideoSelect"/>
    </section>
  </div>
</template>

<script>
import axios from 'axios'
import SearchBar from '@/components/SearchBar'
import VideoList from '@/components/VideoList'
import VideoDetail from '@/components/VideoDetail'
// KEY 같은 민감한 정보는 코드에 넣으면 안된다! 환경 변수(따로 파일에다가 분리)
const API_KEY = process.env.VUE_APP_YOUTUBE_API_KEY
const API_URL = 'https://www.googleapis.com/youtube/v3/search'

export default {
  name: 'App',
  data: function () {
    return {
      inputValue: '',
      videos: [],
      selectedVideo: '', //VideoDetail.vue로 보내고,
    }
  },
  components: {
    SearchBar,
    VideoList,
    VideoDetail,
  },
  methods: {
    onInputSearch: function (inputText) {
      // console.log('데이터가 SearchBar로부터 올라왔다.')
      // console.log(inputText)
      this.inputValue= inputText
      // part(필수), key(필수), q(검색어), type(video만) 매개 변수를 요청에 넣어서 보냄
      const params = {
        key: API_KEY,
        part: 'snippet',
        type: 'video',
        q: this.inputValue
      }
      axios.get(API_URL, {
        params, 
      })
      .then((res) => {
        console.log(res)
        // console.log(res.data.items)
        this.videos = res.data.items
        console.log(this.videos)
        // 선택 된 비디오가 없다면
        if (!this.selectedVideo) {
          this.selectedVideo = this.videos[0]
        }
      })
      .catch((err) => {
        console.log(err)
      })
    },
    onVideoSelect: function (video) {
      this.selectedVideo = video
    },
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

section,
header {
  width: 80%;       /* 전체 너비의 80% */
  margin: 0 auto;   /* 양 옆 margin을 균등하게 배분*/
  padding: 1rem 0;  /* 위, 아래 padding */
}

section {
  display: flex;  /* Detail, List를 가로 배치 */
}

</style>
