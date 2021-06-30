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
        // publicKey: {
        //     type: String,
        //     required: true
        // },
        // debug: {
        //     type: Boolean,
        //     default: false
        // },
        // style: Object,
        // cardholder: String,
        // localization: Object,
        // ready: Function,
        // frameActivated: Function,
        // frameFocus: Function,
        // frameBlur: Function,
        // frameValidationChanged: Function,
        // paymentMethodChanged: Function,
        // cardValidationChanged: Function,
        // cardSubmitted: Function,
        // cardTokenized: Function,
        // cardTokenizationFailed: Function
    },
    methods: {
        initializeFrames: function() {
            let config = {
                publicKey: this.publicKey,
                debug: this.debug,
                style: this.style,
                cardholder: this.cardholder,
                localization: this.localization,
                ready: this.ready,
                frameActivated: this.frameActivated,
                frameFocus: this.frameFocus,
                frameBlur: this.frameBlur,
                frameValidationChanged: this.frameValidationChanged,
                paymentMethodChanged: this.paymentMethodChanged,
                cardValidationChanged: this.cardValidationChanged,
                cardSubmitted: this.cardSubmitted,
                cardTokenized: this.cardTokenized,
                cardTokenizationFailed: this.cardTokenizationFailed,
            };

            // Frames throws an error if the cardholder object is mentioned but not defined
            // To avoid this we remove the property completely if not set as a prop.
            if (!this.config.cardholder) {
                delete config.cardholder;
            }

            // Frames throws an error if the localization object is mentioned but not defined
            // To avoid this we remove the property completely if not set as a prop.
            if (!this.config.localization) {
                delete config.localization;
            }

            if (window.Frames) {
                window.Frames.init(config);
            } else {
                console.error(
                    `Frames was used before the script (from the CDN) was loaded completely`
                );
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
            return window.Frames.submitCard();
        },
        addEventHandler: function(event, handler) {
            window.Frames.addEventHandler(event, handler);
        },
        removeEventHandler: function(event, handler) {
            window.Frames.removeEventHandler(event, handler);
        },
        removeAllEventHandlers: function(event) {
            window.Frames.removeAllEventHandlers(event);
        },
        enableSubmitForm: function() {
            window.Frames.enableSubmitForm();
        }
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
};
</script>