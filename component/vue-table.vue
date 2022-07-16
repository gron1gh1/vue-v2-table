<template>
  <div>
    <div class="p-2">
      <table>
        <thead>
        <tr
            v-for="headerGroup in table.getHeaderGroups()"
            :key="headerGroup.id"
        >
          <th
              v-for="header in headerGroup.headers"
              :key="header.id"
              :colSpan="header.colSpan"
          >
            {{header.column.id}}
          </th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="row in table.getRowModel().rows" :key="row.id">
          <td v-for="cell in row.getVisibleCells()" :key="cell.id">
            {{cell.row.original[cell.column.columnDef.accessorKey]}}
          </td>
        </tr>
        </tbody>
      </table>
      <div class="h-4" />
      <button @click="rerender" class="border p-2">Rerender</button>
      <button @click="defaultData[0].age=1231" class="border p-2">Change</button>
    </div>
  </div>
</template>

<script lang="ts">
import {
  Column,
  ColumnDef,
  flexRender,
  getCoreRowModel,
  useVueTable,
} from '@/lib/vue-table'
import { ref,defineComponent } from '@vue/composition-api';
type Person = {
  firstName: string
  lastName: string
  age: number
  visits: number
  status: string
  progress: number
}

export default defineComponent({
  setup() {
    const defaultData: Person[] = [
      {
        firstName: 'tanner',
        lastName: 'linsley',
        age: 24,
        visits: 100,
        status: 'In Relationship',
        progress: 50,
      },
      {
        firstName: 'tandy',
        lastName: 'miller',
        age: 40,
        visits: 40,
        status: 'Single',
        progress: 80,
      },
      {
        firstName: 'joe',
        lastName: 'dirte',
        age: 45,
        visits: 20,
        status: 'Complicated',
        progress: 10,
      },
    ]

    const defaultColumns: ColumnDef<Person>[] = [
      {
        accessorKey: 'firstName',
        id:"첫번째",
        cell: info => info.getValue(),
        footer: info => info.column.id,
      },
      {
        accessorKey: 'lastName',
        id:"마지막",
        cell: info => info.getValue(),
        footer: info => info.column.id,
      },
      {
        accessorKey: 'age',
        id:"나이",
        cell: info => info.getValue(),
        footer: info => info.column.id,
      },

    ]


    const data = ref(defaultData)

    const rerender = () => {
      data.value = defaultData
    }

    const table = useVueTable({
      get data() {
        return data.value
      },
      columns: defaultColumns,
      getCoreRowModel: getCoreRowModel(),
    });
    console.log("AA",table);
    return {
      defaultData,
      table,
      rerender,
      flexRender,
    }
  }
})
</script>

<style lang="scss" scoped>
.box {
  margin-bottom: 10px;
}
html {
  font-family: sans-serif;
  font-size: 14px;
}

table {
  border: 1px solid lightgray;
}

tbody {
  border-bottom: 1px solid lightgray;
}

th {
  border-bottom: 1px solid lightgray;
  border-right: 1px solid lightgray;
  padding: 2px 4px;
}

tfoot {
  color: gray;
}

tfoot th {
  font-weight: normal;
}
</style>
