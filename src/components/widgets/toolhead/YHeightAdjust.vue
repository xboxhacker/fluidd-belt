<template>
  <v-row
    align="start"
    justify="end"
  >
    <v-col
      cols="6"
      class="text-right"
    >
      <app-btn-toggle
        v-model="moveDistance"
        mandatory
        dense
        :disabled="!klippyReady"
      >
        <app-btn
          v-for="(value, i) in yAdjustValues"
          :key="i"
          small
          class="px-1"
          :disabled="!klippyReady"
          min-width="36"
          :value="value"
        >
          {{ value }}
        </app-btn>
      </app-btn-toggle>
      <div
        class="mt-1"
        :class="{ 'text--disabled': !klippyReady }"
      >
        <span class="secondary--text">Y Offset&nbsp;</span>
        <span>{{ yHomingOrigin.toFixed(3) }}mm</span>
      </div>
    </v-col>
    <v-col cols="6">
      <v-row
        justify="space-between"
        no-gutters
        class="mr-n1"
      >
        <v-col
          cols="4"
          class="pr-1"
        >
          <app-btn
            :loading="hasWait($waits.onYAdjust)"
            :disabled="!klippyReady"
            small
            block
            @click="sendYAdjustGcode('+')"
          >
            <v-icon small>
              $zUp
            </v-icon>
          </app-btn>
        </v-col>
        <v-col
          cols="4"
          class="pr-1"
        >
          <app-btn
            :loading="hasWait($waits.onYAdjust)"
            :disabled="!klippyReady"
            small
            block
            @click="sendYAdjustGcode('-')"
          >
            <v-icon small>
              $zDown
            </v-icon>
          </app-btn>
        </v-col>
        <v-col
          cols="4"
          class="pr-1"
        >
          <app-btn
            :disabled="!klippyReady || printerPrinting || yHomingOrigin === 0"
            small
            block
            @click="handleYOffsetApply"
          >
            <v-icon small>
              $save
            </v-icon>
          </app-btn>
        </v-col>
      </v-row>
      <v-row
        no-gutters
        class="mt-2 mr-n1"
      >
        <v-col>
          <app-btn
            :disabled="!klippyReady"
            small
            block
            @click="resetYOffset"
          >
            Reset Y Offset
          </app-btn>
        </v-col>
      </v-row>
    </v-col>
  </v-row>
</template>

<script lang="ts">
import { Component, Mixins } from 'vue-property-decorator'
import StateMixin from '@/mixins/state'

@Component({})
export default class YHeightAdjust extends Mixins(StateMixin) {
  moveDistanceValue: number | null = null

  get yHomingOrigin (): number {
    return this.$typedState.printer.printer.gcode_move.homing_origin[1]
  }

  get yAdjustValues (): number[] {
    return this.$typedState.config.uiSettings.general.zAdjustDistances
  }

  get moveDistance (): number {
    return this.moveDistanceValue || this.yAdjustValues[0]
  }

  set moveDistance (value: number) {
    this.moveDistanceValue = value
  }

  /**
   * Send a Y adjust gcode script with MOVE=1 to move the toolhead.
   */
  sendYAdjustGcode (direction: '+' | '-') {
    const gcode = `SET_GCODE_OFFSET Y_ADJUST=${direction}${this.moveDistance} MOVE=1`
    this.sendGcode(gcode, this.$waits.onYAdjust)
  }

  /**
   * Reset Y offset to zero.
   */
  resetYOffset () {
    this.sendGcode('SET_GCODE_OFFSET Y=0 MOVE=0', this.$waits.onYAdjust)
  }

  /**
   * Save the current Y offset to config.
   * Calls Y_OFFSET_SAVE_CONFIG macro if available.
   */
  handleYOffsetApply () {
    this.sendGcode('Y_OFFSET_SAVE_CONFIG')
  }
}
</script>
