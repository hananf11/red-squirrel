<template>
  <main>
    <div class="flex items-center gap-3">
      <v-label
        size="sm"
        variant="text"
        text="database name"
      />
      <span class="text-primary-600 font-bold text-2xl">/</span>
      <v-button
        size="sm"
        variant="text"
        text="home"
      />
    </div>
  </main>
  <main>
    <span class="col-span-4">Dialog</span>
    <v-dialog
      mode="success"
      body="Well done!"
    />
    <v-dialog
      mode="error"
      body="You suck!"
    />
    <v-dialog
      mode="confirm"
    >
      <template #body>
        Are you sure?
        <v-input />
      </template>
    </v-dialog>
    <v-dialog
      mode="prompt"
    />
  </main>
  <main>
    <span class="col-span-5">Message</span>
    <v-message body="Hello, World!" />
    <v-message
      status="success"
      body="Hello, World!"
    />
    <v-message
      status="error"
      body="Hello, World!"
    />
    <v-message
      status="warning"
      body="Hello, World!"
    />
    <v-message
      status="info"
      body="Hello, World!"
    />
  </main>
  <main class="grid-cols-5">
    <span class="col-span-full">
      Number
    </span>
    <template
      v-for="attrs, i in [
        { size: 'sm' },
        {},
        { hollow: true },
        { size: 'lg' },
      ]"
      :key="`${i}-label-outer`"
    >
      <template
        v-for="variant, j in [
          '',
          'primary',
          'success',
          'error',
          'warning',
        ]"
        :key="`${i}-${j}-label-inner`"
      >
        <v-number
          v-model="testNumbers[0][0]"
          :variant="variant"
          v-bind="attrs"
        />
      </template>
    </template>
    <!-- <v-number /> -->
  </main>
  <main>
    <span class="col-span-full">
      Label
    </span>
    <v-field
      label="field test"
      :input="'input'"
      :issues="[
        { message: 'error message', type: 'error' },
        { message: 'warning message', type: 'warning' },
        { message: 'success message', type: 'success' },
      ]"
      required
    />
  </main>
  <main class="grid-cols-6">
    <span class="col-span-full">
      Label
    </span>
    <template
      v-for="attrs, i in [
        { size: 'sm' },
        {},
        { hollow: true },
        { size: 'lg' },
      ]"
      :key="`${i}-label-outer`"
    >
      <template
        v-for="variant, j in [
          '',
          'primary',
          'success',
          'error',
          'warning',
          'text'
        ]"
        :key="`${i}-${j}-label-inner`"
      >
        <v-label
          text="Hello, World"
          :variant="variant"
          v-bind="attrs"
        />
      </template>
    </template>
  </main>
  <main class="lg:grid-cols-5 grid-cols-4">
    <span class="col-span-full">
      Group
    </span>
    <template
      v-for="attrs, i in [
        { size: 'sm' },
        { vertical: true },
        { hollow: true },
        { disabled: true },
        { size: 'lg' },
      ]"
      :key="`${i}-group-outer`"
    >
      <template
        v-for="variant, j in [
          '',
          'primary',
          'success',
          'error',
          'warning',
        ]"
        :key="`${i}-${j}-group-outer`"
      >
        <v-group
          v-model="testGroups[i][j]"
          v-bind="attrs"
          :variant="variant"
        >
          <v-checkbox />
          <v-input
            placeholder="Hey im in a group"
          />
          <v-label text="group" />
        </v-group>
      </template>
    </template>
  </main>
  <main
    class="lg:grid-cols-6 grid-cols-4"
  >
    <span class="col-span-full">
      Checkbox
    </span>
    <!-- default -->
    <template
      v-for="attrs, i in [
        {},
        { disabled: true },
      ]"
      :key="`${i}-checkbox-outer`"
    >
      <template
        v-for="variant, j in [
          '',
          'primary',
          'success',
          'error',
          'warning',
          'text'
        ]"
        :key="`${i}-${j}-checkbox-outer`"
      >
        <v-checkbox
          v-model="testChecks[i][j]"
          text="Hello, World"
          :variant="variant"
          v-bind="attrs"
        />
      </template>
    </template>
  </main>
  <main class="lg:grid-cols-6 grid-cols-4">
    <span class="col-span-full">
      Button
    </span>
    <template
      v-for="attrs, i in [
        {},
        { disabled: true },
      ]"
      :key="`${i}-button-outer`"
    >
      <template
        v-for="variant, j in [
          '',
          'primary',
          'success',
          'error',
          'warning',
          'text'
        ]"
        :key="`${i}-${j}-button-outer`"
      >
        <v-button
          text="Hello, World"
          :variant="variant"
          v-bind="attrs"
        />
      </template>
    </template>
  </main>
  <main class="lg:grid-cols-5 grid-cols-3">
    <span class="col-span-full">
      Input
    </span>
    <template
      v-for="attrs, i in [
        { },
        { placeholder: 'placeholder test!' },
        { disabled: true },
      ]"
      :key="`${i}-input-outer`"
    >
      <template
        v-for="variant, j in [
          '',
          'primary',
          'success',
          'error',
          'warning',
        ]"
        :key="`${i}-${j}-input-inner`"
      >
        <v-input
          v-model="testInputs[i][j]"
          v-bind="attrs"
          :variant="variant"
        />
      </template>
    </template>
  </main>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'
import * as componentsAll from '@/components/Core'

export default defineComponent({
  components: {
    ...componentsAll
  },
  setup () {
    const size = ref(10)
    const testInputs = ref([
      ['0-0', '0-1', '0-2', '0-3', '0-4'],
      ['', '', '', '', ''],
      ['2-0', '2-1', '2-2', '2-3', '2-4']
    ])
    const testChecks = ref([
      [true, true, true, true],
      [true, true, true, true]
    ])
    const testGroups = ref([
      [
        [true, '0-0', undefined],
        [true, '0-1', undefined],
        [true, '0-2', undefined],
        [true, '0-3', undefined],
        [true, '0-4', undefined]
      ],
      [
        [true, '1-0', undefined],
        [true, '1-1', undefined],
        [true, '1-2', undefined],
        [true, '1-3', undefined],
        [true, '1-4', undefined]
      ],
      [
        [true, '2-0', undefined],
        [true, '2-1', undefined],
        [true, '2-2', undefined],
        [true, '2-3', undefined],
        [true, '2-4', undefined]
      ],
      [
        [true, '3-0', undefined],
        [true, '3-1', undefined],
        [true, '3-2', undefined],
        [true, '3-3', undefined],
        [true, '3-4', undefined]
      ],
      [
        [true, '4-0', undefined],
        [true, '4-1', undefined],
        [true, '4-2', undefined],
        [true, '4-3', undefined],
        [true, '4-4', undefined]
      ],
      [
        [true, '5-0', undefined],
        [true, '5-1', undefined],
        [true, '5-2', undefined],
        [true, '5-3', undefined],
        [true, '5-4', undefined]
      ]
    ])
    const testNumbers = [
      [0, 0, 0, 0, 0],
      [0, 0, 0, 0, 0],
      [0, 0, 0, 0, 0],
      [0, 0, 0, 0, 0],
      [0, 0, 0, 0, 0]
    ]
    return { size, testInputs, testChecks, testGroups, testNumbers }
  }
})
</script>

<style lang="postcss" scoped>
main { @apply grid gap-5 items-center justify-items-center p-10; }
</style>
