<template>
  <div>
    <div class="header">
      <ul class="header-button-left">
        <li v-if="tabStatus == !0">Cancel</li>
      </ul>
      <ul class="header-button-right">
        <li v-if="tabStatus == 1" @click="tabStatus++">Next</li>
        <li v-if="tabStatus == 2" @click="publish">Save</li>
      </ul>
      <img src="./assets/logo.png" class="logo" />
    </div>

    <Container :selectedFilter="selectedFilter" :feed="feed" :tabStatus="tabStatus" :tempImgUrl="tempImgUrl" @write="feedText = $event" />

    <button v-if="tabStatus == 0" @click="more">더보기</button>

    <div v-if="tabStatus == 0" class="footer">
      <ul class="footer-button-plus">
        <input @change="upload" type="file" id="file" class="inputfile" />
        <label for="file" class="input-plus">+</label>
      </ul>
    </div>

  </div>

</template>

<script>
import Container from './components/Container';
import datalist from './assets/data.js';
import axios from 'axios';

export default {
  name: 'App',
  data() {
    return {
      feed : datalist,
      moreCount : 0,
      tabStatus : 0,
      tempImgUrl : '',
      feedText: '',
      selectedFilter: '',
    }
  },
  mounted() {
    this.emitter.on('applyFilter', (data) => {
      this.selectedFilter = data;
      console.log(this.selectedFilter);
    })
  },
  components: {
    Container,
  },
  methods: {
    publish() {
      let myPost = {
        name: "Kim Hyun",
        userImage: "https://placeimg.com/100/100/arch",
        postImage: this.tempImgUrl,
        likes: 36,
        date: "May 15",
        liked: false,
        content: this.feedText,
        filter: this.selectedFilter
      };
      this.feed.unshift(myPost);
      this.tabStatus = 0;
    },
    more(){
      axios.get(`https://codingapple1.github.io/vue/more${this.moreCount}.json`)
          .then((result) => {
            // console.log(result);
            // this.feed.push(result.data)
            this.feed.push(result.data);
            this.moreCount++;
          })
    },
    upload(e) {
      let tempImage = e.target.files;
      console.log(tempImage);
      let imgUrl = URL.createObjectURL(tempImage[0]);
      console.log(imgUrl);
      this.tempImgUrl = imgUrl;
      this.tabStatus = 1;
    }
  }
}
</script>

<style>
@import 'style.css';

</style>
