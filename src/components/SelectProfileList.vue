<script>
import { computed, reactive, toRefs } from 'vue'

export default {
  props: {
    position: {
      type: Number,
      required: true
    }
  },
  setup(props, { emit }) {
    const state = reactive({
      names: ['ben', 'cassidy', 'jason', 'kenny', 'phil', 'tara']
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
    <label :for="selectProfileId">Profile #{{ position + 1 }}</label>
    <select
      :name="selectProfileId"
      :id="selectProfileId"
      @input="updateSelection"
    >
      <option disabled selected value>-- Select a Name --</option>
      <option
        v-for="name in names"
        :key="`position-${position}-${name}`"
        :value="name"
      >
        {{ name }}
      </option>
    </select>
  </div>
</template>

<style></style>
