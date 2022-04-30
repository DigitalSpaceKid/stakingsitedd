<template>
  <div class="nes-container with-title mb-10">
    <p class="title">Reward</p>
    <p>Welcome to our rewards page. Here we explain how our staking rewards are distributed. </p>
    <p class="mt-5">
      Staked Dogs earn 3 $HALO per day
    </p>
    <p class="mt-5">
      After 10 consecutive days your Dog ascends to Tier 1, obtaining a 1.33x multiplier or 4 $HALO a day
    </p>

    <p class="mt-5">
      After 30 consecutive days your Dog ascends to Tier 2, obtaining a 2x multiplier or 6 $HALO a day
    </p>

    <p class="mt-5">
      After 77 consecutive days your Dog ascends to Tier 3, obtaining a 3x multiplier or 9 $HALO a day
    </p>

    <p class="mt-5">
      This way we will reward long-term holders while maintaining longevity for $HALO. 
    </p>

        <p class="mt-5"> </p>
          <router-link to="farmer">
        <button class="nes-btn is-primary">Stake NFTs</button>
      </router-link>
    </div>
</template>

<script lang="ts">
import { defineComponent, onMounted, ref, watch } from 'vue';
import ConfigPane from '@/components/ConfigPane.vue';
import useWallet from '@/composables/wallet';
import useCluster from '@/composables/cluster';
import TestMint from '@/components/gem-farm/TestMint.vue';
import { initGemFarm } from '@/common/gem-farm';
import InitFarm from '@/components/gem-farm/InitFarm.vue';
import { PublicKey } from '@solana/web3.js';
import { stringifyPKsAndBNs } from '@gemworks/gem-farm-ts';
import AuthorizeFunder from '@/components/gem-farm/AuthorizeFunder.vue';
import FundCancelLock from '@/components/gem-farm/FundCancelLock.vue';
import RefreshFarmer from '@/components/gem-farm/RefreshFarmer.vue';
import TreasuryPayout from '@/components/gem-farm/TreasuryPayout.vue';
import TheWhitelist from '@/components/gem-farm/BankWhitelist.vue';
import UpdateFarm from '@/components/gem-farm/UpdateFarm.vue';
import FarmDisplay from '@/components/gem-farm/FarmDisplay.vue';

export default defineComponent({
  components: {
    FarmDisplay,
    UpdateFarm,
    TheWhitelist,
    TreasuryPayout,
    RefreshFarmer,
    FundCancelLock,
    AuthorizeFunder,
    InitFarm,
    TestMint,
    ConfigPane,
  },
  setup() {
    const { wallet, getWallet } = useWallet();
    const { cluster, getConnection } = useCluster();

    let gf: any;
    watch([wallet, cluster], async () => {
      gf = await initGemFarm(getConnection(), getWallet()!);
      await findFarmsByManager(getWallet()!.publicKey!);
    });

    //needed in case we switch in from another window
    onMounted(async () => {
      if (getWallet() && getConnection()) {
        gf = await initGemFarm(getConnection(), getWallet()!);
        await findFarmsByManager(getWallet()!.publicKey!);
      }
    });

    // --------------------------------------- farm locator
    const foundFarms = ref<any[]>([]);
    const farm = ref<string>();
    const farmAcc = ref<any>();
    const currentFarmIndex = ref<number>(0);
    const isLoading = ref<boolean>(true);

    //whenever we change the farm, we update the index/account
    watch(farm, (newFarm: any) => {
      updateFarmByPk(newFarm);
    });

    const updateFarmByPk = (newFarm: string) => {
      const idx = foundFarms.value.findIndex(
        (ff) => ff.publicKey.toBase58() === newFarm
      );
      currentFarmIndex.value = idx;
      farmAcc.value = foundFarms.value[idx].account;
    };

    const findFarmsByManager = async (manager: PublicKey) => {
      foundFarms.value = await gf.fetchAllFarmPDAs(manager);
      console.log('Found farms:', stringifyPKsAndBNs(foundFarms.value));

      if (foundFarms.value.length) {
        farm.value =
          foundFarms.value[currentFarmIndex.value].publicKey.toBase58();
        //yes this is needed here, as sometimes farm.value stays same, but we want to rerender anyway
        updateFarmByPk(farm.value!);
      }
      isLoading.value = false;
    };

    // --------------------------------------- rest
    const showNewFarm = ref<boolean>(false);

    const handleNewFarm = async (newFarm: string) => {
      showNewFarm.value = false;
      await findFarmsByManager(getWallet()!.publicKey!);
      farm.value = newFarm;
    };

    const handleUpdateFarm = async () => {
      await findFarmsByManager(getWallet()!.publicKey!);
    };

    const refreshFarms = async () => {
      await findFarmsByManager(getWallet()!.publicKey!);
    };

    return {
      isLoading,
      wallet,
      foundFarms,
      farm,
      farmAcc,
      handleNewFarm,
      handleUpdateFarm,
      showNewFarm,
      refreshFarms,
    };
  },
});
</script>

<style scoped></style>
