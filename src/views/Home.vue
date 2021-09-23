<template>
  <div class="home">
    <h1 v-if="stats">{{ calculatedPrice }}</h1>
    <h2 v-if="stats">{{ this.stats.total }}</h2>
    <p>{{ locationName }}</p>
    <div class="wrapper">
      <Bar
        v-for="reward in rewards"
        :key="reward.hour"
        :max="max"
        :value="reward.total"
      />
    </div>
  </div>
</template>

<script>
// import Percentage from "@/components/Percentage.vue";
import Bar from "@/components/Bar.vue";
import { subDays, format, differenceInHours } from "date-fns";

const formatDate = `yyyy-MM-dd'T'kk:mm:ss'Z'`;
const now = format(new Date(), formatDate);
const yesterday = format(subDays(new Date(), 1), formatDate);

// const days30 = format(subDays(new Date(), 30), formatDate);

export default {
  name: "Home",
  components: {
    // Percentage,
    Bar,
  },
  data() {
    return {
      price: null,
      stats: [],
      locationName: "",
      rewards: [],
      hours: {},
    };
  },
  computed: {
    calculatedPrice: function () {
      if (!this.stats.total) return "...";
      return this.price
        ? `£${(this.price * this.stats.total).toFixed(2)}`
        : this.stats.total;
    },
    max: function () {
      return Math.max(...this.rewards.map((item) => item.total)) || 0;
    },
  },
  methods: {
    formatData(rewards) {
      const withHours = rewards.map((item) => ({
        ...item,
        hour: differenceInHours(new Date(), new Date(item.timestamp)),
      }));
      console.log("withHours", withHours);
      this.rewards = [...Array(24)]
        .map((_, key) => ({
          hour: key,
          total: withHours
            .filter((reward) => reward.hour === key)
            .reduce((a, b) => a + b.amount, 0),
        }))
        .reverse();
    },
    async getData() {
      try {
        let response = await fetch(
          `https://api.helium.io/v1/hotspots/${this.$route.query.id}/rewards/sum?min_time=${yesterday}&max_time=${now}`
        );
        const result = await response.json();
        this.stats = result.data;
      } catch (error) {
        console.log("error", error);
      }
    },
    async getLastDayRewards() {
      try {
        const response = await fetch(
          `https://api.helium.io/v1/hotspots/${this.$route.query.id}/rewards?min_time=${yesterday}&max_time=${now}`
        );
        const result = await response.json();
        const cursor = result.cursor;
        const response2 = await fetch(
          `https://api.helium.io/v1/hotspots/${this.$route.query.id}/rewards?min_time=${yesterday}&max_time=${now}&cursor=${cursor}`
        );
        const result2 = await response2.json();
        this.formatData(result2.data);
      } catch (error) {
        console.log("error", error);
      }
    },
    async getCurrency() {
      let response = await fetch(
        `https://api.coingecko.com/api/v3/simple/price?ids=helium&vs_currencies=gbp`
      );
      const result = await response.json();
      this.price = result.helium.gbp;
    },
  },
  created() {
    this.getData();
    this.getCurrency();
    this.getLastDayRewards();
  },
};
</script>

<style>
.home {
}

.home h1 {
  font-size: 80px;
  margin: 70px 0 0;
  color: #fff;
}

.home h2 {
  margin: -10px 0 80px;
  color: #fff;
}

.wrapper {
  position: relative;
  height: 150px;
  display: flex;
  justify-content: center;
}
</style>
