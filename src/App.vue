<template>
  <div className="App">
    <div id="payment-form">
      <input
        class="card-name"
        type="text"
        placeholder="Cardholder name"
        name="cc-name"
        autocomplete="cc-name"
      />
      <CardNumber />
      <div class="date-and-code">
        <ExpiryDate />
        <Cvv />
      </div>
      <button class="pay" v-on:click="submitCard">Pay Now</button>
    </div>
  </div>
</template>

<script>
import CardNumber from "./components/CardNumber.vue";
import ExpiryDate from "./components/ExpiryDate.vue";
import Cvv from "./components/Cvv.vue";

export default {
  name: "App",
  components: {
    CardNumber,
    ExpiryDate,
    Cvv,
  },
  props: {
    // onCardTokenized: Function
  },
  data() {
    return {};
  },
  mounted: function() {
    this.$nextTick(function() {
      // remove event handlers to avoid event duplication
      window.Frames.removeAllEventHandlers(window.Frames.Events.CARD_SUBMITTED);
      window.Frames.removeAllEventHandlers(
        window.Frames.Events.CARD_TOKENIZATION_FAILED
      );
      window.Frames.removeAllEventHandlers(window.Frames.Events.CARD_TOKENIZED);
      window.Frames.removeAllEventHandlers(
        window.Frames.Events.CARD_VALIDATION_CHANGED
      );
      window.Frames.removeAllEventHandlers(
        window.Frames.Events.FRAME_ACTIVATED
      );
      window.Frames.removeAllEventHandlers(window.Frames.Events.FRAME_BLUR);
      window.Frames.removeAllEventHandlers(window.Frames.Events.FRAME_FOCUS);
      window.Frames.removeAllEventHandlers(
        window.Frames.Events.FRAME_VALIDATION_CHANGED
      );
      window.Frames.removeAllEventHandlers(
        window.Frames.Events.PAYMENT_METHOD_CHANGED
      );
      window.Frames.removeAllEventHandlers(window.Frames.Events.READY);
      window.Frames.init({
        publicKey: "pk_test_4296fd52-efba-4a38-b6ce-cf0d93639d8a",
        cardTokenized: this.onCardTokenized,
        localization: {
          cardNumberPlaceholder: "•••• •••• •••• ••••",
          expiryMonthPlaceholder: "MM",
          expiryYearPlaceholder: "YY",
          cvvPlaceholder: "•••",
        },
        debug: true,
        style: {
          base: {
            paddingLeft: "1rem",
            border: "1px solid #9b9b9b",
            borderRadius: "5px",
            color: "#263238",
            fontSize: "12px",
            fontStyle: "normal",
            fontFamily: "'Open Sans', Open Sans, Helvetica, Arial, sans-serif",
            fontWeight: "normal",
            lineHeight: "16px",
            letterSpacing: "0.22px",
          },
          focus: {
            border: "1px solid #41a5dd",
            boxShadow: "0px 0px 2px #41a5dd",
            backgroundColor: "#fff",
          },
          invalid: {
            border: "1px solid #D96830",
          },
          placeholder: {
            base: {
              fontSize: "12px",
            },
            focus: {
              fontSize: "14px",
            },
          },
        },
      });
    });
  },
  methods: {
    onCardTokenized(e) {
      console.log("Token details:", e);
      alert(`The card token: ${e.token} \nDetails in console. `);
    },
    submitCard: function() {
      window.Frames.cardholder.name = document.querySelector(
        ".card-name"
      ).value;
      window.Frames.submitCard();
    },
  },
};
</script>

<style></style>
