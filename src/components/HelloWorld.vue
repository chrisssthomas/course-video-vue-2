<template>
  <div class="wrapper">
    <div class="heading">
      <h2>Selected videos</h2>
      <p>Select which order videos will play within the course.</p>

      <input
        type="text"
        v-model="search"
        placeholder="Video title"
        class="search"
      />
    </div>

    <div class="video-picker">
      <div class="video-wrapper">
        <div v-for="video in filteredList" :key="video.uri">
          <div
            class="video"
            :class="{
              'video-selected': isSelected(video.id),
            }"
            @click="selectVideo(video)"
          >
            <div>
              <img
                :src="video.thumbnail"
                :alt="videos.name"
                class="video-thumbnail"
              />

              <div class="video-title">
                <abbr :title="video.name">{{ video.title }}</abbr>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="divider"></div>
      <div>
        <h3>Positions:</h3>
        {{ positionsWithVideos }}
        <div
          v-for="(video, index) in positions"
          :key="video.id"
          class="position"
        >
          <p>{{ index + 1 }}: {{ video.title }}</p>
          <div>
            <button
              @click="moveVideoUpInPosition(index)"
              class="position-button"
            >
              up
            </button>
            <button
              @click="moveVideoDownInPosition(index)"
              class="position-button"
            >
              down
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import videoJson from "/videos.json";

export default {
  data() {
    return {
      value: "",
      search: "",
      currentVideo: "",
      selectedVideos: [],
      positions: [],
      videos: videoJson,
    };
  },
  methods: {
    selectVideo(video) {
      if (!this.selectedVideos.includes(video.id)) {
        this.selectedVideos.push(video.id);
        this.positions.push(video);
      } else {
        this.selectedVideos.splice(this.selectedVideos.indexOf(video.id), 1);
        this.positions.splice(this.selectedVideos.indexOf(video), 1);
      }
    },
    isSelected(id) {
      return this.selectedVideos.includes(id);
    },
    arraymove(arr, fromIndex, toIndex) {
      var element = arr[fromIndex];
      arr.splice(fromIndex, 1);
      arr.splice(toIndex, 0, element);
    },
    moveVideoDownInPosition(index) {
      this.arraymove(this.positions, index, index + 1);
    },
    moveVideoUpInPosition(index) {
      if (index == 0) {
        return;
      }
      this.arraymove(this.positions, index, index - 1);
    },
  },
  computed: {
    filteredList() {
      return this.videos.filter((video) => {
        return video.title.toLowerCase().includes(this.search.toLowerCase());
      });
    },
    positionsWithVideos() {
      return this.positions.map((x, i) => [{ videoId: x.id, position: i }]);
    },
  },
};
</script>

<style scoped>
.wrapper {
  border: 1px solid #e5e5e5;
}
.position {
  border: 1px solid gray;
  margin: 1rem 0;
  display: flex;
  justify-content: space-between;
}

.heading {
  padding: 1.45rem;
}

.search {
  border-radius: 40px;
  border: none;
  padding: 0.5rem 1rem;
  background: #dedede;
  width: 255px;
}
.search::placeholder {
  color: rgba(0, 0, 0, 0.4);
}
.divider {
  width: 100%;
  height: 1px;
  background: #e5e5e5;
}
.circular {
  width: 90px;
  height: 90px;
  background-size: cover;
  border-radius: 50px;
  -webkit-border-radius: 50px;
  -moz-border-radius: 50px;
}
.video-picker {
  overflow: scroll;
}
.current-video {
  margin: 0.5rem 0;
}
.selected-video {
  padding: 0 0.5rem;
}
.video-wrapper {
  display: flex;
  /* max-width: 850px; */
  flex-wrap: wrap;
}
.video {
  flex: 1 1 0px;
  margin: 1.45rem;
  cursor: pointer;
  min-width: 285px;
  max-width: 285px;
  border-radius: 6px;
}
.video-selected {
  z-index: 10;
  box-shadow: inset 0 0 0 4px rgba(32, 45, 32, 0.5);
}
.video-thumbnail {
  height: 170px;
  width: 100%;
  margin-left: auto;
  margin-right: auto;
  /* background-repeat: no-repeat;
  background-size: 100% 100%; */
  z-index: -1;
  border-radius: 6px 6px 0px 0px;
}
.video-title {
  border-width: 0px 1px 1px 1px;
  border-color: #e5e5e5;
  border-style: solid;
  border-radius: 0 0 3px 3px;
  height: 100%;
  padding: 1rem;
  margin-top: -5px;
  font-weight: 700;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
@media screen and (max-width: 800px) {
  .video-wrapper {
    justify-content: center;
  }
  .video {
    max-width: 200px;
  }
}
</style>
