<template>
  <div class="app-accordion" :class="{ 'is-open': open }">
    <div class="app-accordion__subject" @click="open = !open">
      <slot name="title"/>
      <div class="app-accordion__icon">
        <ae-icon name="plus" fill="primary" size="2rem"/>
      </div>
    </div>
    <transition name="slide-fade">
      <div v-show="open" class="app-accordion__content">
        <slot name="body"/>
      </div>
    </transition>
  </div>
</template>

<script>
import AeIcon from '@aeternity/aepp-components/dist/ae-icon'

export default {
  name: 'app-accordion',
  components: {
    AeIcon
  },
  props: ['title', 'accordion'],
  data: function () {
    return { open: false }
  },
  methods: {
    toggle () {
      this.open = !this.open
    }
  }
}
</script>
<style lang="scss" scoped>
.app-accordion {
  margin-top: $spacer-m;
  padding-bottom: $spacer-m;
  width: 100%;
  text-align: left;
  &:not(:last-child){
    border-bottom: 2px solid $grey;
  }

  &__subject {
    display: flex;
    cursor: pointer;
    justify-content: space-between;
    align-items: center;
  }

  &__content {
    height: auto;
    & a {
      font-weight: bold;
    }
    & ul > li{
      margin: .75rem 0;
      margin-left: 2.5rem
    }
  }
}

.slide-fade-enter-active,
.slide-fade-leave-active {
  transition: all .3s ease-in-out;
  @include only-phone {
      transition: all .2s ease-in-out;
  }
  max-height: 300px;
}
.slide-fade-enter, .slide-fade-leave-to {
  max-height: 0;
  opacity: 0;
}
.app-accordion__icon {
  transition: transform .3s ease-in-out;
  height: 2rem;
  width: 2rem;
}
.is-open .app-accordion__icon {
  transform: rotate(45deg);
}
</style>
