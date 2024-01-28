<template>
  <div class="q-pa-lg">
    <div class="row justify-between items-center q-mb-md">
      <h3 class="text-h6"></h3>
      <div>
        <q-btn
          label="+ New Part"
          color="primary"
          @click="isModalFormPart = true"
        ></q-btn>
      </div>
    </div>

    <TablePart :items="items" />

    <ModalFormPart
      v-model="isModalFormPart"
      @add="(item) => handleEmitForm(item)"
    />
  </div>
</template>

<script setup lang="ts">
import { ref, watch } from "vue";
import ModalFormPart from "./components/ModalFormPart.vue";
import TablePart from "./components/TablePart.vue";
import { ItemTy } from "./type";

const isModalFormPart = ref(false);
const file = ref(null);

const items = ref<ItemTy[]>([]);

function handleEmitForm(item: ItemTy) {
  if (item) {
    items.value.push(item);
  }
}
</script>

<style scoped></style>
