<!-- ❗️BETA -->

This project is a minimal Vue wrapper of [Checkout.com Frames](https://docs.checkout.com/integrate/frames). This version only supports the [multiple iframes](https://docs.checkout.com/integrate/frames/frames-customization-guide#Framescustomizationguide-Multipleiframes) configuration.

<!-- # :rocket: Install

```bash
npm install frames-vue
``` -->

# :globe_with_meridians: Load the CDN script

Make sure that you load the Checkout.com CDN script before you mount any Frames components. You can add this, for example, in your _index.html_ file.

```html
<script src="https://cdn.checkout.com/js/framesv2.min.js"></script>
```

If you use server-side rendering, such as with _Nuxt.js_, you can add this inside your **head** object **script** array:

```js
head: {
    script: [
      {
        src: "https://cdn.checkout.com/js/framesv2.min.js",
      },
    ],
  }
```

<!-- # :sparkles: Import the components

```js
import { Frames, CardNumber, ExpiryDate, Cvv } from "frames-vue";
``` -->

# :book: Example Usage

```js
<template>
  <div id="payment-form">
    <Frames
      :config="config"
      @ready="ready"
      @frameFocus="frameFocus"
      @cardTokenized="cardTokenized"
    />
    <CardNumber />
    <ExpiryDate />
    <Cvv />
    <button id="pay-button" @click="submitCard">Pay Now</button>
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
        debug: true,
        publicKey: "pk_test_7d8d24fc-ffdb-4efc-b945-a19847ce319a",
        localization: {
          cardNumberPlaceholder: "Card number",
          expiryMonthPlaceholder: "MM",
          expiryYearPlaceholder: "YY",
          cvvPlaceholder: "CVV",
        },
        style: {
          base: {
            fontSize: "17px",
          },
        },
      },
    };
  },
  methods: {
    cardTokenized(e) { console.log(`Card token: ${e.token}`) },
    ready(e) { console.log("ready", e) },
    frameFocus(e) { console.log("frameFocus", e) },
    submitCard() { Frames.submitCard() },
  },
};
</script>
```
