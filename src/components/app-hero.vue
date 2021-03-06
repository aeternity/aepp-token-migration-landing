<template>
  <header class="app-hero">
    <app-content container wide none>
      <div class="app-hero__inner-wrapper">
        <section class="app-hero__info">
          <div class="app-hero__info__text">
            <slot name="text"/>
          </div>
          <div class="app-hero__info__buttons">
            <slot name="buttons"/>
          </div>
        </section>
        <figure class="app-hero__media" >
          <figure class="app-hero__media_galaxy">
            <img
              :style="{ top: '-' + galaxy + 'px'}"
              :src="require('../assets/graphics/galaxy-phase-2.jpg')" alt="mainnet launch">
          </figure>
        </figure>
      </div>
    </app-content>
    <div class="app-hero__count">
      <div class="app-hero__count-wrapper">
        <p class="token-count">{{burnedBalance | reduceDecimals | formatBalance}}<span class="token-count__currency">AE</span></p>
        <p class="token-count-info">migrated so far</p>
      </div>
      <div class="app-hero__line"></div>
    </div>
  </header>
</template>
<script>
import Web3 from 'web3'
import AppContent from '@/sections/app-content.vue'
let $web3
const abi = [{
  'constant': true,
  'inputs': [
    {
      'name': '_owner',
      'type': 'address'
    }
  ],
  'name': 'balanceOf',
  'outputs': [
    {
      'name': 'balance',
      'type': 'uint256'
    }
  ],
  'payable': false,
  'type': 'function'
}]

$web3 = new Web3(new Web3.providers.HttpProvider(process.env.VUE_APP_WEB3_PROVIDER_URL))
let aeTokenContract = new $web3.eth.Contract(abi, process.env.VUE_APP_AE_TOKEN_CONTRACT)
let height = 0
async function getBurnedBalance () {
  let balance = await aeTokenContract
    .methods
    .balanceOf(process.env.VUE_APP_AE_TOKEN_BURNER)
    .call()
  return Web3.utils.fromWei(balance)
}
export default {
  name: 'app-hero',
  props: {
    title: String
  },
  data () {
    return {
      burnedBalance: null,
      height: 0,
      galaxy: 0
    }
  },
  async created () {
    this.burnedBalance = await getBurnedBalance()
  },
  mounted () {
    window.addEventListener('scroll', this.animateGalaxy, { passive: true })
  },
  destroy () {
    window.removeEventlistener('scroll', () => this.animateGalaxy)
  },
  filters: {
    reduceDecimals: function (value) {
      if (!value) return ''
      value = parseFloat(value)
      return value.toFixed(2)
    },
    formatBalance: function (value) {
      if (!value) return ''
      value = parseFloat(value)
      return value.toLocaleString('en-US')
    }
  },
  methods: {
    animateGalaxy () {
      let start = window.pageYOffset || document.documentElement.scrollTop
      this.galaxy = start > height ? start += 100 : start += 100
      height = start <= 0 ? 0 : start
    }
  },
  components: {
    AppContent
  }
}
</script>
<style lang="scss" scoped>

.app-hero {
  position: relative;
  height: 100%;
  background-color: $skyblue;
  display: flex;
  text-align: left;
  display: flex;
  flex-direction: column;

  &__inner-wrapper {
  min-height: 100vh;
  display: flex;
  @include only-phone{
      padding-top: 6rem;
      padding-bottom: 4rem;
    }
  }
  &__media {
    width: 40%;
    margin-left: auto;

    @include only-phone{
      width: 30%;
    }
      &_galaxy {
        position: absolute;
        top: 25%;
        width: 100%;
        right: -70%;
        transform: translateY(-25%);
        @include only-phone{
          top: 50%;
          transform: translateY(-50%);
          width: 100%;
          right: -75%;
        }
        & > img {
          position: absolute;
          z-index: 0;
          max-width: 50rem;
          border-radius: 50%;
        }
      }
  }

  &__info {
    margin: auto 0;
    width: 70%;
    max-width: 50rem;
    @include only-phone {
      width: 100%;
    }
    @include only-tablet {
      width: 100%;
    }
    &__text {
      @include only-phone {
        max-width: 100%;
        width: 100%;
      }
    }
    &__buttons{
      display: flex;
      flex-direction: column;
      flex-wrap: wrap;
      margin-top: $spacer-xl;
        @include only-phone {
          justify-content: center;
          align-items: center;
          margin-right: -25%;
        }
        @include tablet-and-desktop {
          flex-direction: row;
        }
    }
  }

  &__count {
    min-height: 75vh;
    position: relative;
    background-color: $white;
    z-index: 1;
    left: 0;
    width: 100%;
    display: flex;

    &-wrapper {
      margin: auto;
      background-color: $white;
      padding: 8rem 0;
      @include only-phone{
        padding: 4rem 0 3.5rem;
      }
      text-align: center;
    }
  }

  &__line {
    width: 2px;
    background-color: $magenta;
    position: absolute;
    top: -2.5rem;
    height: 5rem;
    z-index: 1;
    left: 50%;
    transform: translate(-50%);
      @include tablet-and-desktop {
        top: -3rem;
        height: 9rem;
      }
  }
}

.token-count{
  font-weight: bold;
  line-height: 1em;
  margin: 0;
  &-info {
    margin: 0;
    @include font-size(s);
    @include tablet-and-desktop {
      @include font-size(l);
    }
  }
  color: $magenta;
    @include font-size(wow);
  @include only-phone {
    @include font-size(xxl);
  }

  &__currency {
     @include font-size(s);
     margin-left: .2em;
     @include tablet-and-desktop {
        @include font-size(l);
        font-weight: 500;
      }
  }
}
.wrapper {
  position: absolute;
  width: 100%;
}
.number {
  position: fixed;
  top: 1rem;
  right: 1rem;
  z-index: 1000;
}

</style>
