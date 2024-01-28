<template>
  <div>
    <q-table
      title="Master Parts changeover matrix"
      flat
      bordered
      :rows="props.items"
      :columns="columns"
      color="primary"
      row-key="name"
    >
      <template v-slot:top-right>
        <div class="q-gutter-sm">
          <q-btn color="primary" label="Import" no-caps />
          <q-btn
            color="primary"
            icon-right="archive"
            label="Export to csv"
            no-caps
            @click="exportTable"
          />
        </div>
      </template>

      <template v-slot:body="props">
        <q-tr :props="props">
          <q-td v-for="col in props.cols" :key="col.name" :props="props">
            <span v-if="col.name == 'actions'">
              <q-btn color="amber" label="Edit" size="sm"></q-btn>
            </span>

            <span v-else> {{ col.value }} </span>
          </q-td>
        </q-tr>
      </template>
    </q-table>
  </div>
</template>

<script setup lang="ts">
import { exportFile, useQuasar } from "quasar";
import { ItemTy } from "../type";

const props = defineProps({
  items: Array<ItemTy>,
});

const columns: any = [
  {
    name: "part_name",
    required: true,
    label: "#",
    align: "left",
    format: (val: any) => `${val}`,
    sortable: true,
    field: "part_name",
  },
  {
    name: "part_a",
    align: "center",
    label: "Part A",
    field: "part_a",
    sortable: true,
  },
  {
    name: "part_b",
    align: "center",
    label: "Part B",
    field: "part_b",
    sortable: true,
  },
  {
    name: "part_c",
    align: "center",
    label: "Part C",
    field: "part_c",
    sortable: true,
  },
  {
    name: "actions",
    label: "",
    field: "actions",
  },
];

const $q = useQuasar();

function wrapCsvValue(val: any, formatFn: any, row: any) {
  let formatted = formatFn !== void 0 ? formatFn(val, row) : val;

  formatted =
    formatted === void 0 || formatted === null ? "" : String(formatted);

  formatted = formatted.split('"').join('""');
  /**
   * Excel accepts \n and \r in strings, but some other CSV parsers do not
   * Uncomment the next two lines to escape new lines
   */
  // .split('\n').join('\\n')
  // .split('\r').join('\\r')

  return `"${formatted}"`;
}

function exportTable() {
  // naive encoding to csv format
  const content = [columns.map((col) => wrapCsvValue(col.label))]
    .concat(
      props.items.map((row) =>
        columns
          .map((col) =>
            wrapCsvValue(
              typeof col.field === "function"
                ? col.field(row)
                : row[col.field === void 0 ? col.name : col.field],
              col.format,
              row
            )
          )
          .join(",")
      )
    )
    .join("\r\n");

  const status = exportFile(`part.csv`, content, {
    encoding: "utf-8",
    mimeType: "text/csv",
    byteOrderMark: "\uFEFF",
  });

  if (status !== true) {
    $q.notify({
      message: "Browser denied file download...",
      color: "negative",
      icon: "warning",
    });
  }
}
</script>
