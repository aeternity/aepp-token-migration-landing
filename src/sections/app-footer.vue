<template>
  <footer class="app-footer">
    <app-content container dark wide>
      <div class="app-footer__branding">
        <a href="https://aeternity.com/" target="_blank">
          <ae-logo/>
        </a>
        <blockquote class="title title-category app-footer__quote">
          Scalable smart contracts <br>
          interfacing with real-world data
        </blockquote>
      </div>
      <div class="app-footer__details">
        <small>&#xa9; aeternity <time>{{ year }}</time></small>
        <nav class="footer-nav">
          <div class="footer-nav__internal">
            <router-link :to="{ name:'Imprint' }" class="footer-nav__link">Imprint</router-link>
            <router-link :to="{ name:'Legal' }" class="footer-nav__link">Legal</router-link>
            <router-link :to="{ name:'Terms & Services' }" class="footer-nav__link">Terms</router-link>
          </div>
          <div class="footer-nav__external">
            <a href="https://forum.aeternity.com/t/token-migration-phase-1-support-and-faq/1411" target="_blank" class="footer-nav__link footer-nav__link">Visit our forum</a>
            <a href="https://aeternity.com/" target="_blank" class="footer-nav__link footer-nav__link_last">aeternity.com</a>
          </div>
        </nav>
      </div>
    </app-content>
  </footer>
</template>
<script>
import AeLogo from '@/components/ae-logo.vue'
import AppContent from '@/sections/app-content.vue'

export default {
  name: 'app-footer',
  computed: {
    year () {
      return new Date().getFullYear()
    }
  },
  components: {
    AeLogo,
    AppContent
  }
}
</script>
<style lang="scss" scoped>
  .app-footer {
    background-color: $darkblue;
    //padding: 0 0 $spacer-max ;
  @include font-size(xs);
    color: $white;
    &__details {
      display: flex;
      @include only-phone {
        flex-wrap: wrap;
      }
    }
    &__quote {
      margin-top: $spacer-m;
    }

    &__branding {
      margin-bottom: $spacer-xxl;
    }
  }
.footer-nav{
  display: flex;
  width: 100%;
  @include only-phone {
    flex-direction: column;

  }
  &__external {
      margin-right: 0;
      @include tablet-and-desktop {
        margin-left: auto;
      }
     }
  &__link{
     color: $white;
     display: inline-block;
     margin-right: $spacer-l;
     font-weight: normal;

      @include only-phone {
        display: block;
        margin-right: 0;
        margin-bottom: $spacer-m;
      }

     &_last {
      margin: 0;
     }
  }
}
small {
  display: block;
  font-size: 1em;
  min-width: 8em;
  opacity: .7;
  margin-right: $spacer-l;
  @include only-phone {
    margin-bottom: $spacer-m;
  }
}
time {
  display: inline;
}
</style>
