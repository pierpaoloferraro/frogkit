<style lang="stylus">
  @require '../stylus/mixins/dialogue.styl'

  .fk-dialogue
    dialogue-mixin()

  .noscroll
    overflow hidden
</style>

<template>
  <transition appear enter-active-class="fk-dialogue__animation-in" leave-active-class="fk-dialogue__animation-out">
    <Flex v-if="visible" justify="center" align="center" class="fk-dialogue" :class="classObject">
      <section class="fk-dialogue__frame" v-click-outside="dismiss">
        <a v-if="!requireAction" @click="dismiss" class="fk-dialogue__close">
          <Icon icon="close-light" color="orange" />
        </a>
        <!-- <Flex column justify="start" align="center" > -->
          <slot />
        <!-- </Flex> -->
      </section>
    </Flex>
  </transition>
</template>

<script>
  import clickOutside from '../directives/click-outside'

  export default {
    name: 'Dialogue',
    directives: {
      clickOutside,
    },
    model: {
      prop: 'visible',
      event: 'change',
    },
    props: {
      visible: {
        type: Boolean,
        default: false,
      },
      dark: {
        type: Boolean,
        default: false,
      },
      requireAction: {
        type: Boolean,
        default: false,
      },
    },
    computed: {
      classObject() {
        return {
          'fk-dialogue--dark': !!this.dark,
        }
      },
    },
    mounted() {
      document.body.classList.toggle('noscroll', true)
    },
    destroyed() {
      document.body.classList.toggle('noscroll', false)
    },
    methods: {
      close() {
        this.$emit('change', false)
      },
      dismiss() {
        if (!this.requireAction) {
          this.close()
        }
      },
    },
  }
</script>
