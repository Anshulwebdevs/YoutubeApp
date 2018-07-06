<template>
  <div class="container">
    <span class="text-center"><h3>Youtube Search</h3></span>
    <SearchBar @termChange="onTermChange"></SearchBar>
    <div class="row">
    <VideoDetail :video="selectedVideo"></VideoDetail>
    <VideoList :videos="videos" @videoSelect="onVideoSelect"></VideoList>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import SearchBar from './components/SearchBar';
import VideoList from './components/VideoList';
import VideoDetail from './components/VideoDetail';
const API_KEY = 'AIzaSyAHuTTiQul72c39CW6z1h3nhvdEgVlCdiw';

export default {
  
  name: 'App',
  components: {
    SearchBar,
    VideoList,
    VideoDetail
  },
  data(){
    return {
      videos:[],
      selectedVideo:null
    }
  },
  methods:{
    onVideoSelect(video){
      //console.log(video);
      this.selectedVideo = video;
    },
    onTermChange(searchTerm){
       if(searchTerm === '') {
        this.videos = [];
        return;
       };
      axios.get('https://www.googleapis.com/youtube/v3/search',{
        params:{
          key: API_KEY,
          type:'video',
          part:'snippet',
          q:searchTerm
        }
      }).then(response => {
        this.videos = response.data.items;
        this.selectedVideo = this.videos[0];
      });
    }

  }
}
</script>

<style>
body{
  margin-top: 20px; 
}
</style>
