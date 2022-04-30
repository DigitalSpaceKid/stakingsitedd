<template>
  <div class="nes-container with-title mb-10">
    <p class="title">Welcome</p>
    <p>Welcome to the Divine Dogs staking website. </p>
    <p class="mt-5">
      The goal of Divine Dogs is to provide sustainable, long-term passive
      income and utility to our holders as well as DeFi knowledge. By analyzing the top market cap NFT
      projects with a native token, and other sources of passive income, we aim
      to create a hybrid of tokenomics and royalty rewards, backed by proof of
      work and dedication from our team. 
    </p>
    <p class="mt-5">
      That's why we will use our own native token $HALO, which will be used in our eco system. We have set
      up numerous of utilities for $HALO to be used. 
    </p>

  </div>
  <div class="flex flex-row">
    <div class="nes-container with-title flex-1 mr-5">
      <p class="title">Reward</p>
      <p class="mb-5">Checkout how we distribute our staking rewards.</p>
      <router-link to="reward">
        <button class="nes-btn is-primary">Reward</button>
      </router-link>
    </div>
    <div class="nes-container with-title flex-1 ml-5">
      <p class="title">Stake your NFTs</p>
      <p class="mb-5">
        Here you can stake your Divine Dogs to earn some $HALO.
      </p>
      <router-link to="farmer">
        <button class="nes-btn is-primary">Stake NFTs</button>
      </router-link>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { GEM_FARM_PROG_ID } from '@gemworks/gem-farm-ts';
export default defineComponent({
  setup() {
    return {
      GEM_FARM_PROG_ID,
    };
  },
});
</script>
<style scoped></style>
