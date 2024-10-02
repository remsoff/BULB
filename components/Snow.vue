<script setup lang="ts">
import { createSnow, showSnow } from 'pure-snow.js'

const snow = useSnow()
const showDialog = ref(false)

const { isWinter, checkYearlyMessage } = snow
const { snowEnabled } = storeToRefs(snow)
watch(snowEnabled, val => showSnow(val))

tryOnMounted(() => {
  if (isWinter) {
    if (checkYearlyMessage()) {
      showDialog.value = true
      snowEnabled.value = true
    }
    createSnow()
    showSnow(snowEnabled.value)
  }
})

function toggleAndClose() {
  showDialog.value = false
  snowEnabled.value = false
}
</script>

<template>
  <Dialog
    v-model:visible="showDialog"
    pt:root:class="md:w-40em"
    :header="$t('ui.dialogs.end_of_year_celebrations.header')"
    modal
    :draggable="false"
  >
    <p>{{ $t('ui.dialogs.end_of_year_celebrations.summary') }}</p>

    <template #footer>
      <Button :label="$t('ui.dialogs.end_of_year_celebrations.disable_and_close')" severity="secondary" text @click="toggleAndClose()" />
      <Button :label="$t('ui.dialogs.end_of_year_celebrations.close')" @click="showDialog = false" />
    </template>
  </Dialog>

  <Teleport to="body">
    <div v-if="isWinter" id="snow" />
  </Teleport>
</template>

<style lang="scss">
#snow {
  position: fixed;
  top: 0;
  z-index: 999999;
  height: 100vh !important;
  width: 100vw !important;
  pointer-events: none;
}

.snowflake {
  position: absolute;
  width: 10px;
  height: 10px;
  background: #eee;
  border-radius: 50%;
  filter: drop-shadow(0 0 10px #ddd);
}
</style>
