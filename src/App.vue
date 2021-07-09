<template>
    <div id="payment-form">
      <Frames
          :config="config"
          @ready="ready"
          @frameFocus="frameFocus"
          @cardTokenized="cardTokenized"
          />
      <CardNumber />
      <div class="date-and-code">
        <ExpiryDate />
        <Cvv />
      </div>
      <button class="pay" v-on:click="submitCard">Pay Now</button>
    </div>
</template>

<script>
import Frames from "./Frames.vue";
import CardNumber from "./components/CardNumber.vue";
import ExpiryDate from "./components/ExpiryDate.vue";
import Cvv from "./components/Cvv.vue";

export default {
  name: "App",
  components: {
    Frames,
    CardNumber,
    ExpiryDate,
    Cvv,
  },
  data() {
    return {
      config: {
        debug: false,
        publicKey: 'pk_test_6e40a700-d563-43cd-89d0-f9bb17d35e73',
        localization: {
          cardNumberPlaceholder: '•••• •••• •••• ••••',
          expiryMonthPlaceholder: 'MM',
          expiryYearPlaceholder: 'YY',
          cvvPlaceholder: '•••',
        }
      }
    };
  },
  methods: {
    cardTokenized(e) {
      console.log(`The card token: ${e.token}`);
    },
    ready(e) {
      console.log("ready", e);
    },
    frameFocus(e) {
      console.log("frameFocus",e);
    },
    submitCard: function () {
      Frames.submitCard();
    },
  },
};
</script>

<style>
iframe {
  /* This fixes a mobile Safari bug */
  height: 38px !important;
}

#payment-form {
  width: 280px;
  margin: 0 auto;
}

.date-and-code {
  display: flex;
  margin-bottom: 8px;
}

.date-and-code > div:nth-child(1) {
  width: 55.715%;
}
.date-and-code > div:nth-child(2) {
  width: 45.719%;
}


.card-number-frame,
.expiry-date-frame,
.cvv-frame {
  flex: 1 1 auto;
  padding-left: 40px;
}

.frame--activated {
  opacity: 1;
  border: solid 1px #13395e;
  border-radius: 3px;
  box-shadow: 0 1px 3px 0 rgba(19, 57, 94, 0.2);
}

.frame--activated.frame--focus {
  border: solid 1px #13395e;
  box-shadow: 0 2px 5px 0 rgba(19, 57, 94, 0.15);
}

.frame--activated.frame--invalid {
  border: solid 1px #d96830;
  box-shadow: 0 2px 5px 0 rgba(217, 104, 48, 0.15);
}
</style>
