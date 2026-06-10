<template>
  <ion-page>
    <ion-content>
      <div class="wrapper">
        <ion-button expand="block" class="refresh-btn" @click="loadData">
          Refresh
        </ion-button>

        <div v-for="coin in coins" :key="coin.id" class="coin-card">
          <div class="top-row">
            <span>Rank</span>
            <span>{{ coin.name }}</span>
            <span>USD</span>
          </div>

          <div class="bottom-row">
            <div class="rank">{{ coin.rank }}</div>

            <div class="symbol">{{ coin.symbol }}</div>

            <div class="price">
              {{ formatPrice(coin.price_usd) }}
            </div>
          </div>
        </div>
      </div>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { IonPage, IonContent, IonButton } from "@ionic/vue";
import axios from "axios";

export default defineComponent({
  name: "HomePage",

  components: {
    IonPage,
    IonContent,
    IonButton,
  },

  data() {
    return {
      coins: [] as any[],
    };
  },

  mounted() {
    this.loadData();
  },

  methods: {
    async loadData() {
      try {
        const response = await axios.get(
          "https://api.coinlore.net/api/tickers/",
        );

        this.coins = response.data.data;
      } catch (error) {
        console.error(error);
      }
    },

    formatPrice(price: string) {
      const num = Number(price);

      if (num >= 1) {
        return num.toLocaleString("en-US", {
          minimumFractionDigits: 2,
          maximumFractionDigits: 2,
        });
      }

      return num.toFixed(6);
    },
  },
});
</script>

<style scoped>
ion-content {
  --background: #d9d9d9;
  overflow-x: hidden;
}

/* =========================
   DESKTOP
========================= */
.wrapper {
  width: 100%;
  max-width: 600px;

  margin: 20px auto;
  padding: 30px;

  background: #ffffff;
  border-radius: 10px;
  box-sizing: border-box;

  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.refresh-btn {
  margin-bottom: 25px;
  font-size: 16px;
}

/* =========================
   COIN CARD
========================= */
.coin-card {
  background: #f7edc9;

  border-left: 1px solid #d6c27d;
  border-right: 1px solid #d6c27d;
  border-bottom: 1px solid #d6c27d;

  padding: 10px;
}

.coin-card:first-of-type {
  border-top: 1px solid #d6c27d;
}

.top-row {
  display: grid;
  grid-template-columns: 60px 1fr 90px;

  font-size: 11px;
  color: #555;

  margin-bottom: 4px;
}

.top-row span:first-child {
  text-align: left;
}

.top-row span:nth-child(2) {
  text-align: center;
}

.top-row span:last-child {
  text-align: right;
}

.bottom-row {
  display: grid;
  grid-template-columns: 60px 1fr 90px;
  align-items: center;
}

.rank {
  text-align: center;
  font-size: 24px;
  font-weight: bold;
}

.symbol {
  text-align: center;
  font-size: 24px;
  font-weight: bold;
}

.price {
  text-align: right;
  font-size: 20px;
  font-weight: 500;
}

/* =========================
   MOBILE
========================= */
@media (max-width: 576px) {
  ion-content {
    --background: #ffffff;
  }

  ion-content::part(scroll)::-webkit-scrollbar {
    width: 2px;
  }

  .wrapper {
    width: 100%;
    max-width: none;

    margin: 0;
    padding: 15px;

    background: transparent;
    border-radius: 0;
    box-shadow: none;
  }

  .refresh-btn {
    margin-bottom: 15px;
    font-size: 14px;
  }
}

/* =========================
   SMALL PHONE
========================= */
@media (max-width: 360px) {
  .top-row {
    grid-template-columns: 50px 1fr 80px;
  }

  .bottom-row {
    grid-template-columns: 50px 1fr 80px;
  }

  .rank {
    font-size: 16px;
  }

  .symbol {
    font-size: 16px;
  }

  .price {
    font-size: 16px;
  }
}

/* =========================
   SMARTWATCH / VERY SMALL
========================= */
@media (max-width: 260px) {
  .wrapper {
    padding: 5px;
    padding-bottom: 20px;
  }

  .top-row {
    grid-template-columns: 30px 1fr 45px;
    font-size: 7px;
  }

  .bottom-row {
    grid-template-columns: 30px 1fr 45px;
  }

  .coin-card {
    padding: 6px;
  }

  .rank {
    font-size: 10px;
  }

  .symbol {
    font-size: 10px;
  }

  .price {
    font-size: 10px;
  }

  .refresh-btn {
    margin-bottom: 10px;
    font-size: 10px;
    min-height: 32px;
  }
}
</style>
