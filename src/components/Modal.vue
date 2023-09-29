<template>
  <div class="overlay" v-if="visible">
    <div class="modal">
      <div class="modal__header">
        <div class="title">{{ title }}</div>
        <button
          type="button"
          class="close-btn"
          @click="$emit('update:visible', false)"
        >
          x
        </button>
      </div>

      <div class="modal__content">
        <slot />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    visible: {
      type: Boolean,
      default: false,
    },
    title: {
      type: String,
      required: true,
    },
  },
};
</script>

<style lang="scss" scoped>
.overlay {
  background: rgba(0, 0, 0, 0.6);
  display: flex;
  justify-content: center;
  align-items: center;

  height: 100vh;
  width: 100%;

  position: absolute;
  left: 0;
  top: 0;

  .modal {
    background: #fff;
    padding: 20px;

    &__header {
      display: flex;
      justify-content: space-between;
      margin-bottom: 20px;

      .close-btn {
        border: none;
        background: transparent;
        cursor: pointer;
      }
    }
  }
}
</style>
