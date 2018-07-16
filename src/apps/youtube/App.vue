<template>
  <div class="container">
    <span class="text-center"><h3>Youtube Search</h3></span>
    <SearchBar @termChange="onTermChange"></SearchBar>
    <div class="row">
    <VideoDetail :video="selectedVideo"></VideoDetail>
    <VideoList :data="videos" @videoSelect="onVideoSelect"  @navigation="onNavigation"></VideoList>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import SearchBar from '../../components/SearchBar';
import VideoList from '../../components/VideoList';
import VideoDetail from '../../components/VideoDetail';
const API_KEY = 'Your Api Key';

export default {
  
  name: 'Youtube',
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
       }
       this.searchTerm = searchTerm;
      axios.get('https://www.googleapis.com/youtube/v3/search',{
        params:{
          key: API_KEY,
          type:'video',
          part:'snippet',
          videoDefinition:'standard',
          q:searchTerm
        }
      }).then(response => {
        // console.log(response.data);
        this.videos = response.data;
        // let next = response.data.nextPageToken || '';
        // let prev = response.data.prevPageToken || '';
        // this.nav = {next, prev};
        this.selectedVideo = this.videos.items[0];
      });
    },
    onNavigation(pageToken){
      // console.log(pageToken,"page token");
      axios.get('https://www.googleapis.com/youtube/v3/search',{
        params:{
          key: API_KEY,
          type:'video',
          part:'snippet',
          videoDefinition:'standard',
          q:this.searchTerm,
          pageToken:pageToken
        }
      }).then(response => {
        this.videos = response.data;
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
