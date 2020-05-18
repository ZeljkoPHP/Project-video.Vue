<template>
    <div class="container"> <!--ubaci enter key click event-->
        <SearchBar @termChange="onTermChange"></SearchBar>
        <div class="row">
            <VideoDetail :video="selectedVideo.video" :key="selectedVideo"></VideoDetail>
            <VideoList @videoSelect="onVideoSelect" :videos="videos"></VideoList>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
import SearchBar from './components/SearchBar';
import VideoList from './components/VideoList';
import VideoDetail from './components/VideoDetail';

const API_KEY = 'AIzaSyA5RyVuumXc9i_e8YJowqWuTnlona-U8rQ';

export default {
    name: 'App',
    components: {
        SearchBar,
        VideoList,
        VideoDetail
    },
    data() {
        return { videos: [], selectedVideo: null };
    },
    methods: {
        onVideoSelect(video) {
            this.selectedVideo = video;
        },
        onTermChange(searchTerm) {
            if(event.keyCode == 13) {
                axios.get('https://www.googleapis.com/youtube/v3/search', {
                    params: {
                        key: API_KEY,
                        type: 'video',
                        part: 'snippet',
                        q: searchTerm
                    }
                })
                .then(response => {
                    this.videos = response.data.items;
                });
            }
        }
    }
};
</script>