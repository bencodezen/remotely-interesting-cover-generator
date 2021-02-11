<script>
import { computed, reactive, toRefs } from 'vue'
import profileImages from '../data/profileImages.json'

export default {
  props: {
    position: {
      type: Number,
      required: true
    }
  },
  setup(props, { emit }) {
    const state = reactive({
      names: profileImages
    })

    const selectProfileId = computed(() => {
      return `select-profile-${props.position}`
    })

    const updateSelection = event => {
      emit('update-selection', {
        name: event.target.value,
        position: props.position
      })
    }

    return {
      ...toRefs(state),
      selectProfileId,
      updateSelection
    }
  }
}
</script>

<template>
  <div>
    <label :for="selectProfileId" style="display: block"
      >Profile #{{ position + 1 }}:
    </label>
    <select
      :name="selectProfileId"
      :id="selectProfileId"
      @input="updateSelection"
    >
      <option disabled selected>-- Select a Name --</option>
      <option
        v-for="(imageUrl, name) in names"
        :key="`position-${position}-${name}`"
        :value="name"
      >
        {{ name }}
      </option>
    </select>
  </div>
</template>

<style></style>
