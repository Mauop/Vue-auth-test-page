<template>
  <div class="modal__mask">
    <div class="modal__wrapper" @click.self="$emit('close')">
      <div class="modal">
        <div class="modal__cont">
          <slot name="header"></slot>
          <slot name="body"></slot>
          <slot name="footer"></slot>
          <slot name="close">
            <div class="modal__close__icon" @click="$emit('close')">
              <svg
                width="15"
                height="15"
                viewBox="0 0 15 15"
                fill="none"
                xmlns="http://www.w3.org/2000/svg"
              >
                <path d="M14 1L1 14M1 1L14 14" stroke="black" stroke-width="2" />
              </svg>
            </div>
          </slot>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  mounted() {
    document.body.style.overflow = 'hidden'
  },
  destroyed() {
    document.body.style.overflow = ''
  }
}
</script>

<style lang="scss" scoped>
@import '@/assets/scss/components/_modal.scss';
</style>
