


<script lang="ts">
import { defineComponent } from 'vue';
import FixedScheduleDisplay from '@/components/gem-farm/FixedScheduleDisplay.vue';
import { parseDate } from '@/common/util';
import numeral from 'numeral';

export default defineComponent({
  components: { FixedScheduleDisplay },
  props: {
    reward: Object,
    farmReward: Object,
    title: String,
  },
  setup() {
    const parseRewardType = (reward: any): string => {
      //returns "variable" or "fixed"
      return Object.keys(reward.rewardType)[0];
    };

    return {
      parseRewardType,
      parseDate,
      numeral,
    };
  },
});
</script>

<style scoped></style>
