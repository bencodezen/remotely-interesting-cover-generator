<script>
import SelectProfileList from './components/SelectProfileList.vue'

import profileImagesData from './data/profileImages.json'
import imagePositionData from './data/imagePosition.json'

export default {
  components: {
    SelectProfileList
  },
  data: () => ({
    baseUrl: 'https://res.cloudinary.com/dzkoxrsdj/image/upload/',
    preferences: {
      modifiers: {
        qualityAuto: true,
        reduceSize: true
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
    },
    updateProfileGrid({ name, position }) {
      this.profileImageGrid[position] = name
    }
  }
}
</script>

<template>
  <main style="max-width: 1024px; margin: 0 auto">
    <h1>Remotely Interesting Cover Generator</h1>
    <div class="main-grid">
      <section>
        <img :src="finalUrl" alt="" style="max-width: 100%" />
      </section>
      <section>
        <h2>Image Properties</h2>
        <form @submit.prevent>
          <div>
            <div>
              <label for="episode-title" style="font-weight: bold"
                >Episode Title</label
              >
            </div>
            <div><input type="text" v-model="episodeTitle" /></div>
          </div>
          <div class="select-profile-grid">
            <SelectProfileList
              v-for="item in [0, 1, 2, 3, 4, 5]"
              :key="`select-profile-${item}`"
              :position="item"
              @update-selection="updateProfileGrid"
            />
          </div>
          <div>
            <input
              type="checkbox"
              v-model="preferences.modifiers.qualityAuto"
            />
            <label for="quality-auto">Automatically Quality and Encoding</label>
          </div>
          <div>
            <input type="checkbox" v-model="preferences.modifiers.reduceSize" />
            <label for="reduce-size">Reduce Image Size</label>
          </div>
        </form>
      </section>
    </div>
    <h2>Final URL</h2>
    <p style="word-wrap: break-word">
      <a :href="finalUrl">{{ finalUrl }}</a>
    </p>
  </main>
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

.main-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
}

.select-profile-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-column-gap: 15px;
  grid-row-gap: 15px;
  padding: 15px;
  margin-bottom: 10px;
}
</style>
