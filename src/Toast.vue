<template>
  <v-snackbar
    :timeout="timeout"
    :color="color"
    :bottom="y === 'bottom'"
    :top="y === 'top'"
    :left="x === 'left'"
    :right="x === 'right'"
    :multi-line = "multiLine"
    :vertical = "vertical"
    v-model="active"
    class="v-application vts"
    :class="classes"
    @click="dismiss"
    role="alert"
  >
    <v-icon
      dark
      left
      v-if="!!icon"
      class="vts__icon"
      :color="iconColor"
    >
      {{ icon }}
    </v-icon>

    <div class="vts__message" :class="{ 'vts__message--padded': showClose && !closeText }" v-html="message"></div>

    <v-btn
      :icon="!closeText"
      :text="!!closeText"
      class="vts__close"
      :class="{ 'vts__close--icon': !closeText }"
      :color="closeColor"
      v-if="showClose"
      @click="close"
    >
      <v-icon v-if="!closeText">close</v-icon>
      <span v-if="!!closeText">{{ closeText }}</span>
    </v-btn>
  </v-snackbar>
</template>

<script>
export default {
  props: {
    x: {
      type: String,
      default: 'right'
    },
    y: {
      type: String,
      default: 'bottom'
    },
    color: {
      type: String,
      default: 'info'
    },
    icon: {
      type: String,
      default: ''
    },
    iconColor: {
      type: String,
      default: ''
    },
    classes: {
      type: [ String, Object, Array ],
      default: ''
    },
    message: {
      type: String,
      default: ''
    },
    timeout: {
      type: Number,
      default: 3000
    },
    dismissable: {
      type: Boolean,
      default: true
    },
    multiLine: {
      type: Boolean,
      default: false
    },
    vertical: {
      type: Boolean,
      default: false
    },
    showClose: {
      type: Boolean,
      default: false
    },
    closeText: {
      type: String,
      default: ''
    },
    closeColor: {
      type: String,
      default: ''
    }
  },

  data: () => ({
    active: false
  }),

  mounted() {
    this.$nextTick(() => this.show())
  },

  watch: {
    active: function (isActive, wasActive) {
      this.$emit('statusChange', isActive, wasActive)
    }
  },

  methods: {
    show() {
      this.active = true
    },

    close() {
      this.active = false
    },

    dismiss() {
      if (this.dismissable) {
        this.close()
      }
    }
  }
}
</script>

<style>
  .vts {
    max-width: none !important;
    width: auto !important;
  }

  .vts .v-snack__content {
    justify-content: flex-start;
  }

  .vts__icon {
    margin-right: 12px;
  }

  .vts__message {
    margin-right: auto;
  }

  .vts__close {
    margin: 0 -8px 0 24px !important;
    justify-self: flex-end;
  }

  .vts.v-snack--vertical .vts__icon {
    margin: 0 0 12px !important;
  }

  .vts.v-snack--vertical .v-snack__content {
    padding-bottom: 16px !important;
  }

  .vts.v-snack--vertical .vts__message--padded {
    padding: 12px 0 0;
  }

  .vts.v-snack--vertical .vts__icon + .vts__message {
    padding-top: 0;
  }

  .vts.v-snack--vertical .vts__close {
    margin: 12px 0 -8px !important;
  }

  .vts.v-snack--vertical .vts__close--icon {
    margin: 0 !important;
    position: absolute;
    right: 4px;
    top: 4px;
    transform: scale(0.75);
    padding: 4px !important;
  }
</style>
