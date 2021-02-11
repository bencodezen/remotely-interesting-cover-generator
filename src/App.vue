<script>
import profileImagesData from './data/profileImages.json'
import imagePositionData from './data/imagePosition.json'

export default {
  data: () => ({
    baseUrl: 'https://res.cloudinary.com/dzkoxrsdj/image/upload/',
    preferences: {
      modifiers: {
        qualityAuto: false,
        reduceSize: false
      }
    },
    profileImagesList: profileImagesData,
    episodeTitle: 'Placeholder',
    profileImageGrid: [],
    imagePosition: imagePositionData
  }),
  computed: {
    backgroundImage() {
      return 'v1599191282/ri-social-2.png'
    },
    encodedTitle() {
      return window.encodeURI(this.episodeTitle)
    },
    finalUrl() {
      return (
        this.baseUrl +
        this.modifiers +
        this.title +
        this.profileImages +
        this.backgroundImage
      )
    },
    profileImages() {
      return (
        this.profileImageGrid
          .map((name, index) => {
            return this.encodeProfileImage(name, index)
          })
          .join('/') + '/'
      )
    },
    font() {
      return 'l_text:futura_64'
    },
    modifierList() {
      return [this.qualityAuto, this.reduceSize].filter(item => item !== '')
    },
    modifiers() {
      if (this.modifierList.length > 0) {
        return this.modifierList.join(',') + '/'
      } else {
        return ''
      }
    },
    qualityAuto() {
      return this.preferences.modifiers.qualityAuto ? 'q_auto' : ''
    },
    reduceSize() {
      return this.preferences.modifiers.reduceSize ? 'f_auto' : ''
    },
    title() {
      return (
        this.font +
        `:${this.encodedTitle},w_1275,c_fit,co_rgb:FFFFFF,g_south_west,x_110,y_1055/`
      )
    }
  },
  methods: {
    encodeProfileImage(name, index) {
      const position = this.imagePosition[index]

      return (
        'l_fetch:' +
        window.btoa(this.profileImagesList[name]) +
        `,g_north_west,w_190,h_190,r_max,x_${position.x},y_${position.y}`
      )
    }
  }
}
</script>

<template>
  <img alt="Vue logo" src="./assets/logo.png" />

  <h2>Dynamic Image</h2>
  <img :src="finalUrl" alt="" style="max-width: 100%" />

  <p>Ben {{ encodeProfileImage('ben', 0) }}</p>
  <p>Profile Grid: {{ profileImages }}</p>

  <h2>Final URL</h2>
  <p>{{ finalUrl }} - {{ finalUrl.length }}</p>

  <h2>Profile Image Grid</h2>
  <p>{{ profileImageGrid }}</p>

  <h2>URL Components</h2>
  <p>Base Url: {{ baseUrl }}</p>
  <form @submit.prevent>
    <div>
      <input type="checkbox" v-model="preferences.modifiers.qualityAuto" />
      <label for="quality-auto">Automatically Quality and Encoding</label>
    </div>
    <div>
      <input type="checkbox" v-model="preferences.modifiers.reduceSize" />
      <label for="reduce-size">Reduce Image Size</label>
    </div>
    <div>
      <label for="episode-title">Episode Title</label>
      <input type="text" v-model="episodeTitle" />
    </div>
    <div>
      <label for="profile-1">Profile #1</label>
      <select name="profile-1" id="profile-1" v-model="profileImageGrid[0]">
        <option value="ben">Ben</option>
        <option value="jason">Jason</option>
      </select>
    </div>
    <div>
      <label for="profile-2">Profile #2</label>
      <select name="profile-2" id="profile-2" v-model="profileImageGrid[1]">
        <option value="ben">Ben</option>
        <option value="jason">Jason</option>
      </select>
    </div>
  </form>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>