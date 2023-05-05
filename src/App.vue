<template>
  <div class="h-screen flex flex-col space-y-6 items-center justify-center">
    <div
      class="flex flex-col items-center bg-gray-200 p-6 rounded shadow w-[200px]">
      <div>Selected factsheet:</div>
      <div class="flex space-x-6 font-bold">
        {{ factSheetType }}
      </div>
    </div>

    <div class="flex space-x-6 bg-gray-200 p-6 rounded shadow">
      <button
        @click="factSheetType = 'Application'"
        type="button"
        class="rounded-md bg-indigo-600 px-3.5 py-2.5 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600 transition-opacity"
        :class="{
          'pointer-events-none opacity-70': factSheetType === 'Application'
        }">
        Applications
      </button>
      <button
        @click="factSheetType = 'BusinessCapability'"
        type="button"
        class="rounded-md bg-indigo-600 px-3.5 py-2.5 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600 transition-opacity"
        :class="{
          'pointer-events-none opacity-70':
            factSheetType === 'BusinessCapability'
        }">
        Business Capability
      </button>
    </div>
  </div>
</template>

<script lang="ts" setup>
import '@leanix/reporting'
import { ref, unref, watch } from 'vue'
const factSheetType = ref<'Application' | 'BusinessCapability'>('Application')

const getReportConfiguration = (
  factSheetType: string
): lxr.ReportConfiguration => ({
  facets: [
    {
      // doesn't update facets
      // key: '1',
      // will update facets
      key: new Date().getTime().toString(),
      fixedFactSheetType: factSheetType
    }
  ]
})

watch(factSheetType, (factSheetType) => {
  lx.updateConfiguration(getReportConfiguration(factSheetType))
})

const init = async () => {
  await lx.init()
  await lx.ready(getReportConfiguration(unref(factSheetType)))
}

init()
</script>
