<script setup lang="ts">
import { DataTooltip } from '@/components/ui/data-tooltip'
import { useNodePingDisplay } from '@/composables/useNodePingDisplay'

const props = defineProps<{
  uuid: string
  online: boolean
}>()

const {
  latencyRenderBars,
  lossRenderBars,
  topPingNetworks,
} = useNodePingDisplay(() => props.uuid)
</script>

<template>
  <div class="flex flex-col">
    <!-- 每家运营商一列：上方延迟数值，下方依次是该运营商自己的延迟条与丢包条，上下一一对应 -->
    <div
      v-if="topPingNetworks.length > 0"
      class="grid gap-2 pr-4"
      :style="{ gridTemplateColumns: `repeat(${topPingNetworks.length}, minmax(0, 1fr))` }"
    >
      <div v-for="net in topPingNetworks" :key="net.name" class="flex min-w-0 flex-col gap-[3px]">
        <DataTooltip
          placement="top"
          :content="`${net.name}\n${net.latency}`"
          content-class="whitespace-pre-wrap w-max px-1.5 !leading-[1.2] text-[11px]"
        >
          <div class="truncate text-center text-[10px]">
            <span :class="net.toneClass">{{ net.latency }}</span>
          </div>
        </DataTooltip>
        <div class="flex w-full flex-col gap-[1px]">
          <div
            class="grid h-1 cursor-auto items-end gap-[1px] transition-all hover:h-2.5"
            :style="{ gridTemplateColumns: `repeat(${net.latencyBars.length}, minmax(0, 1fr))` }"
          >
            <DataTooltip
              v-for="bar in net.latencyBars" :key="bar.key" placement="top" :content="bar.tooltip"
              class="h-full w-full" content-class="whitespace-pre-wrap w-max px-1.5 !leading-[1.2] text-[11px]"
            >
              <span
                class="block h-full w-full rounded-[1px] transition-all hover:scale-y-160"
                :class="bar.className"
              />
            </DataTooltip>
          </div>
          <div
            class="grid h-1 cursor-auto items-end gap-[1px] transition-all hover:h-2.5"
            :style="{ gridTemplateColumns: `repeat(${net.lossBars.length}, minmax(0, 1fr))` }"
          >
            <DataTooltip
              v-for="bar in net.lossBars" :key="bar.key" placement="top" :content="bar.tooltip"
              class="h-full w-full" content-class="whitespace-pre-wrap w-max px-1.5 !leading-[1.2] text-[11px]"
            >
              <span
                class="block h-full w-full rounded-[1px] transition-all hover:scale-y-160"
                :class="bar.className"
              />
            </DataTooltip>
          </div>
        </div>
      </div>
    </div>
    <template v-else>
      <div class="truncate">
        N/A
      </div>
      <div class="flex flex-col gap-[1px] w-full pr-4">
        <div class="relative items-center gap-1">
          <div
            class="grid h-1 cursor-auto items-end gap-[1px] transition-all hover:h-2.5"
            :style="{ gridTemplateColumns: `repeat(${latencyRenderBars.length}, minmax(0, 1fr))` }"
          >
            <DataTooltip
              v-for="bar in latencyRenderBars" :key="bar.key" placement="top" :content="bar.tooltip"
              class="h-full w-full" content-class="whitespace-pre-wrap w-max px-1.5 !leading-[1.2] text-[11px]"
            >
              <span
                class="block h-full w-full rounded-[1px] transition-all hover:scale-y-160"
                :class="bar.className"
              />
            </DataTooltip>
          </div>
        </div>
        <div class="relative items-center gap-1">
          <div
            class="grid h-1 cursor-auto items-end gap-[1px] transition-all hover:h-2.5"
            :style="{ gridTemplateColumns: `repeat(${lossRenderBars.length}, minmax(0, 1fr))` }"
          >
            <DataTooltip
              v-for="bar in lossRenderBars" :key="bar.key" placement="top" :content="bar.tooltip"
              class="h-full w-full" content-class="whitespace-pre-wrap w-max px-1.5 !leading-[1.2] text-[11px]"
            >
              <span
                class="block h-full w-full rounded-[1px] transition-all hover:scale-y-160"
                :class="bar.className"
              />
            </DataTooltip>
          </div>
        </div>
      </div>
    </template>
  </div>
</template>
