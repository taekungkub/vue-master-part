<template>
  <div>
    <q-dialog
      :model-value="props.modelValue"
      @update:model-value="(value: Boolean) => $emit('update:modelValue', value)"
    >
      <q-card class="full-width" style="max-width: 500px; border-radius: 12px">
        <q-card-section class="row items-center q-pb-none">
          <div class="text-h6"></div>
          <q-space />
          <q-btn icon="close" flat round dense v-close-popup />
        </q-card-section>

        <q-card-section>
          <q-form
            ref="myForm"
            class="q-col-gutter-md"
            @submit.prevent="onSubmit"
          >
            <q-input
              hide-bottom-space
              label="Part Name"
              outlined
              v-model="formState.part_name"
              :rules="[(val) => !!val || 'required']"
            ></q-input>
            <q-input
              hide-bottom-space
              label="Part A"
              outlined
              v-model="formState.part_a"
              :rules="[(val) => !!val || 'required']"
              type="number"
            ></q-input>
            <q-input
              hide-bottom-space
              label="Part B"
              outlined
              v-model="formState.part_b"
              type="number"
              :rules="[(val) => !!val || 'required']"
            ></q-input>
            <q-input
              hide-bottom-space
              label="Part C"
              outlined
              v-model="formState.part_c"
              type="number"
              :rules="[(val) => !!val || 'required']"
            ></q-input>

            <div>
              <q-btn
                label="Confirm"
                color="primary"
                type="submit"
                class="full-width q-mt-md"
              ></q-btn>
            </div>
          </q-form>
        </q-card-section>
      </q-card>
    </q-dialog>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, reactive } from "vue";
import { QDialog, QForm } from "quasar";

const props = defineProps({
  modelValue: {
    type: Boolean,
  },
});

const emit = defineEmits(["update:modelValue", "add"]);

const formState = reactive({
  part_name: "",
  part_a: "",
  part_b: "",
  part_c: "",
});

async function onSubmit() {
  try {
    emit("add", { ...formState });
    // reset
    formState.part_name = "";
    formState.part_a = "";
    formState.part_b = "";
    formState.part_c = "";
    emit("update:modelValue", false);
  } catch (error: any) {
    emit("update:modelValue", false);
  } finally {
  }
}
</script>

<style scoped></style>
