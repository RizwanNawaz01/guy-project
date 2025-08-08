<template>
  <Card class="max-w-xl mx-auto my-4 p-4 space-y-4">
    <CardHeader>
      <CardTitle>Service Frequency</CardTitle>
    </CardHeader>

    <CardContent>
      <RadioGroup v-model="selectedFrequency" class="space-y-2" :orientation="'vertical'">
        <div
          v-for="option in frequencyOptions"
          :key="option.value"
          class="flex items-center space-x-2"
        >
          <RadioGroupItem :value="option.value" :id="option.value" />
          <label :for="option.value" class="ml-2 block">{{ option.label }}</label>
        </div>
      </RadioGroup>

      <div v-if="selectedFrequency === 'custom'" class="border-t pt-4 mt-4 space-y-4">
        <!-- Days Checkboxes -->
        <div>
          <Label class="font-semibold">Select Days</Label>
          <div class="flex flex-wrap gap-2 mt-2">
            <div class="flex items-center space-x-2" v-for="day in daysOfWeek" :key="day">
              <div class="flex items-center ps-3">
                <input
                  :checked="custom.days.includes(day)"
                  @click="() => toggleDay(day, !custom.days.includes(day))"
                  type="checkbox"
                  class="w-4 h-4 cursor-pointer accent-blue-600 mr-2"
                  value=""
                />
                <label for="vue-checkbox">{{ day }}</label>
              </div>
            </div>
          </div>
        </div>

        <!-- Time Windows -->
        <div>
          <div>
            <p>
              <Checkbox
                :checked="custom.enableTimeWindow"
                @click="custom.enableTimeWindow = !custom.enableTimeWindow"
                class="mr-2"
              />Specify time window(s)
            </p>
          </div>

          <div v-if="custom.enableTimeWindow" class="flex gap-2 mt-2">
            <Input v-model="custom.timeWindowStart" type="time" placeholder="Start Time" />
            <Input v-model="custom.timeWindowEnd" type="time" placeholder="End Time" />
          </div>
        </div>

        <!-- Exact Time -->
        <div>
          <div>
            <p>
              <Checkbox
                :checked="custom.enableExactTime"
                @click="custom.enableExactTime = !custom.enableExactTime"
                class="mr-2"
              />Set exact time
            </p>
          </div>
          <div v-if="custom.enableExactTime" class="mt-2">
            <Input v-model="custom.exactTime" type="time" placeholder="Exact Time" />
          </div>
        </div>
      </div>

      <!-- JSON Output -->
      <div class="mt-6">
        <Label class="block font-semibold">JSON Output</Label>
        <pre
          class="bg-gray-100 p-2 text-sm rounded overflow-x-auto mt-2 dark:bg-black dark:text-white"
          >{{ formattedJSON }}</pre
        >
      </div>
    </CardContent>
  </Card>
</template>

<script setup lang="ts">
import { ref, computed, watch } from 'vue'

import Card from '@/components/ui/card/Card.vue'
import CardHeader from '@/components/ui/card/CardHeader.vue'
import CardTitle from '@/components/ui/card/CardTitle.vue'
import CardContent from '@/components/ui/card/CardContent.vue'
import RadioGroup from '@/components/ui/radio-group/RadioGroup.vue'
import RadioGroupItem from '@/components/ui/radio-group/RadioGroupItem.vue'
import Checkbox from '@/components/ui/checkbox/Checkbox.vue'
import Input from '@/components/ui/input/Input.vue'
import Label from '@/components/ui/label/Label.vue'

// Frequency Options
const frequencyOptions = [
  { value: 'weekly', label: 'Weekly' },
  { value: 'fortnightly', label: 'Fortnightly' },
  { value: 'every4weeks', label: 'Every 4 weeks' },
  { value: 'none', label: 'None' },
  { value: 'custom', label: 'Custom' },
]

// Props
const props = defineProps({
  scheduleConfig: {
    type: String,
    default: '',
  },
})

// Reactive state
const selectedFrequency = ref('fortnightly')
const custom = ref({
  days: [] as string[],
  enableTimeWindow: false,
  timeWindowStart: '',
  timeWindowEnd: '',
  enableExactTime: false,
  exactTime: '',
})

const daysOfWeek = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']

watch(
  () => props.scheduleConfig,
  (newVal) => {
    try {
      if (newVal) {
        const parsed = JSON.parse(newVal)
        selectedFrequency.value = parsed.frequency || 'fortnightly'
        if (parsed.frequency === 'custom') {
          custom.value.days = parsed.days || []
          custom.value.enableTimeWindow = !!parsed.timeWindow
          custom.value.timeWindowStart = parsed.timeWindow?.start || ''
          custom.value.timeWindowEnd = parsed.timeWindow?.end || ''
          custom.value.enableExactTime = !!parsed.exactTime
          custom.value.exactTime = parsed.exactTime || ''
        }
      }
    } catch (error) {
      console.error('Error parsing scheduleConfig:', error)
      selectedFrequency.value = 'fortnightly'
    }
  },
  { immediate: true },
)
// Toggle day selection
const toggleDay = (day: string, checked: boolean) => {
  const idx = custom.value.days.indexOf(day)
  if (checked && idx === -1) custom.value.days.push(day)
  else if (!checked && idx !== -1) custom.value.days.splice(idx, 1)
}

// Computed JSON Output
const formattedJSON = computed(() => {
  const output: any = {
    frequency: selectedFrequency.value,
  }

  if (selectedFrequency.value === 'custom') {
    output.days = custom.value.days

    if (custom.value.enableTimeWindow) {
      output.timeWindow = {
        start: custom.value.timeWindowStart,
        end: custom.value.timeWindowEnd,
      }
    }

    if (custom.value.enableExactTime) {
      output.exactTime = custom.value.exactTime
    }
  }

  return JSON.stringify(output, null, 2)
})
</script>
