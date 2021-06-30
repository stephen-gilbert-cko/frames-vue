<template>
  <div class="frames"></div>
</template>

<script>
export default {
    props: {
        config: {
            type: Object,
            default: () => ({})
        }
    },
    methods: {
        initializeFrames: function() {

            let config = {
                publicKey: this.config.publicKey,
                debug: this.config.debug,
                style: this.config.style,
                cardholder: this.config.cardholder,
                localization: this.config.localization,
                ready: ()=>{this.$emit("ready")},
                frameActivated: (e)=>{this.$emit("frameActivated", e)},
                frameFocus: (e)=>{this.$emit("frameFocus", e)},
                frameBlur: (e)=>{this.$emit("frameBlur", e)},
                frameValidationChanged: (e)=>{this.$emit("frameValidationChanged", e)},
                paymentMethodChanged: (e)=>{this.$emit("paymentMethodChanged", e)},
                cardSubmitted: ()=>{this.$emit("cardSubmitted")},
                cardTokenized: (e)=>{this.$emit("cardTokenized", e)},
                cardTokenizationFailed: (e)=>{this.$emit("cardTokenizationFailed", e)},
            };

            // Frames throws an error if the cardholder object is mentioned but not defined
            // To avoid this we remove the property completely if not set as a prop.
            if (!this.config.cardholder) {
                delete config.cardholder;
            }

            // Frames throws an error if the localization object is mentioned but not defined
            // To avoid this we remove the property completely if not set as a prop.
            if (!this.config.localization) {
                delete this.config.localization;
            }

            if (window.Frames) {
                window.Frames.init(config);
            } else {
                console.error(
                    `Frames was used before the script (from the CDN) was loaded completely`
                );
            }
            
        },
    },
    mounted() {
        const existingScript = document.querySelector(
            `script[src$="https://cdn.checkout.com/js/framesv2.min.js"]`
        );
        if (!existingScript) {
            console.error(
                `Checkout.com CDN not present. Perhaps you forgot to add the CDN script to your index.html file.`
            );
        } else {
            this.initializeFrames();
        }
    },
    beforeDestroy() {
        // remove event handlers to avoid event duplication
        if (window.Frames) {
            window.Frames.removeAllEventHandlers(window.Frames.Events.CARD_SUBMITTED);
            window.Frames.removeAllEventHandlers(window.Frames.Events.CARD_TOKENIZATION_FAILED);
            window.Frames.removeAllEventHandlers(window.Frames.Events.CARD_TOKENIZED);
            window.Frames.removeAllEventHandlers(window.Frames.Events.CARD_VALIDATION_CHANGED);
            window.Frames.removeAllEventHandlers(window.Frames.Events.FRAME_ACTIVATED);
            window.Frames.removeAllEventHandlers(window.Frames.Events.FRAME_BLUR);
            window.Frames.removeAllEventHandlers(window.Frames.Events.FRAME_FOCUS);
            window.Frames.removeAllEventHandlers(window.Frames.Events.FRAME_VALIDATION_CHANGED);
            window.Frames.removeAllEventHandlers(window.Frames.Events.PAYMENT_METHOD_CHANGED);
            window.Frames.removeAllEventHandlers(window.Frames.Events.READY);
        }
    },
    init: function(config) {
        // remove event handlers to avoid event duplication
        window.Frames.removeAllEventHandlers(window.Frames.Events.CARD_SUBMITTED);
        window.Frames.removeAllEventHandlers(window.Frames.Events.CARD_TOKENIZATION_FAILED);
        window.Frames.removeAllEventHandlers(window.Frames.Events.CARD_TOKENIZED);
        window.Frames.removeAllEventHandlers(window.Frames.Events.CARD_VALIDATION_CHANGED);
        window.Frames.removeAllEventHandlers(window.Frames.Events.FRAME_ACTIVATED);
        window.Frames.removeAllEventHandlers(window.Frames.Events.FRAME_BLUR);
        window.Frames.removeAllEventHandlers(window.Frames.Events.FRAME_FOCUS);
        window.Frames.removeAllEventHandlers(window.Frames.Events.FRAME_VALIDATION_CHANGED);
        window.Frames.removeAllEventHandlers(window.Frames.Events.PAYMENT_METHOD_CHANGED);
        window.Frames.removeAllEventHandlers(window.Frames.Events.READY);
        config ? window.Frames.init(config) : window.Frames.init();
    },
    isCardValid: function() {
        return window.Frames.isCardValid();
    },
    submitCard: function() {
        console.log("ajunge")
        window.Frames.submitCard();
    },
    enableSubmitForm: function() {
        window.Frames.enableSubmitForm();
    }
};
</script>