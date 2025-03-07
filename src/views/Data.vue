<template>
  <main>
    <table-data-table
      v-model:selected="selected"
      :headers="columnNames"
      :rows="dataRows"
      :row-id="rowId"
    />
    <table-data-editor
      :selected="
        selected.row !== -1 && selected.col !== -1
          ? {
            ...selected,
            value: dataRows?.[selected.row]?.[columnNames[selected.col]],
            column: columnNames[selected.col],
            rowId: dataRows?.[selected.row]?.[rowId],
          }
          : null"
    />
  </main>
</template>

<script lang="ts">
import TableDataEditor from '@/components/TableDataEditor.vue'
import TableDataTable from '@/components/TableDataTable.vue'
import { State } from '@/store/types'
import { computed, defineComponent, provide, ref } from 'vue'
import { useStore } from 'vuex'

export default defineComponent({
  components: {
    TableDataTable,
    TableDataEditor
  },
  props: {
    name: {
      type: String,
      required: true
    }
  },
  setup (props) {
    const store = useStore<State>()
    const statment = computed(() => store.state.database?.connection.prepare(`SELECT ROWID, * FROM [${props.name}]`))
    const rowId = computed(() => statment.value?.getColumnNames()[0])
    const columnNames = computed(() => statment.value?.getColumnNames().slice(1))
    const dataRows = computed(() => {
      if (statment.value) {
        const output = []
        while (statment.value.step()) {
          const rowObject = statment.value.getAsObject()
          output.push(rowObject)
        }
        statment.value.reset()
        return output
      }
      return undefined
    })
    provide('statment', statment)

    const selected = ref({ row: -1, col: -1 })
    return { columnNames, dataRows, selected, rowId }
  }
})
</script>

<style lang="postcss" scoped>
main {
  @apply grid;
  grid-template-columns: 1fr theme('spacing.72');
  grid-template-rows: 1fr;
  @apply items-stretch;
  @apply overflow-x-auto;
  @apply max-h-full;
}
</style>
