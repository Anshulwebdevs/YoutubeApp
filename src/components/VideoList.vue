<template>
	<div class="col-md-4">
		<ul class="list-group">
			<VideoListItem v-for="video in videos"
			:video="video"
			:key="video.etag"
			@videoSelect="onVideoSelect"
			></VideoListItem>	
		</ul>
	</div>
</template>
<script type="text/javascript">
import VideoListItem from './VideoListItem';
	export default{
		name : 'VideoList',
		props:['videos'],
		components:{
			VideoListItem
		},
		methods:{
			onVideoSelect(video){
				this.$emit('videoSelect',video);
			}
		}
	}
</script>
<style type="text/css">
	
</style>