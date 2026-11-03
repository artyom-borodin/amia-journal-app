<template>
  <Dialog
    :visible="visible"
    modal
    :header="APP_CONSTANTS.UI.LABELS.DOWNLOAD_VEDOMOST_TITLE"
    class="vedomost-modal"
    :closable="!isDownloading"
    :closeOnEscape="!isDownloading"
    :dismissableMask="!isDownloading"
    @update:visible="$emit('update:visible', $event)"
  >
    <form @submit.prevent="handleSubmit" class="form-layout">
      <div class="field">
        <label>{{ APP_CONSTANTS.UI.LABELS.ATTESTATION_FORM }}</label>
        <InputText
          v-model="formData.attestation_form"
          :placeholder="APP_CONSTANTS.UI.PLACEHOLDERS.ENTER_ATTESTATION_FORM"
          class="w-full"
          :disabled="isDownloading"
        />
      </div>

      <div class="field">
        <label>{{ APP_CONSTANTS.UI.LABELS.TEACHER_FULL_NAME }}</label>
        <InputText
          v-model="formData.teacher_name"
          :placeholder="APP_CONSTANTS.UI.PLACEHOLDERS.ENTER_TEACHER_NAME"
          class="w-full"
          :disabled="isDownloading"
        />
      </div>

      <div class="field">
        <label>{{ APP_CONSTANTS.UI.LABELS.ATTESTATION_DATE }}</label>
        <DatePicker
          v-model="formData.attestation_date"
          :placeholder="APP_CONSTANTS.UI.PLACEHOLDERS.SELECT_DATE"
          showIcon
          iconDisplay="input"
          class="w-full"
          :disabled="isDownloading"
        />
      </div>

      <div class="field">
        <label>{{ APP_CONSTANTS.UI.LABELS.VEDOMOST_SEMESTER }}</label>
        <InputText
          v-model="formData.semester"
          :placeholder="APP_CONSTANTS.UI.PLACEHOLDERS.ENTER_SEMESTER"
          class="w-full"
          :disabled="isDownloading"
        />
      </div>

      <div class="field">
        <label>{{ APP_CONSTANTS.UI.LABELS.DEAN_NAME }}</label>
        <InputText
          v-model="formData.dean_name"
          :placeholder="APP_CONSTANTS.UI.PLACEHOLDERS.ENTER_DEAN_NAME"
          class="w-full"
          :disabled="isDownloading"
        />
      </div>

      <div class="dialog-footer">
        <Button
          :label="APP_CONSTANTS.UI.LABELS.CANCEL"
          icon="pi pi-times"
          text
          severity="secondary"
          :disabled="isDownloading"
          @click="$emit('update:visible', false)"
        />
        <Button
          type="submit"
          :label="APP_CONSTANTS.UI.LABELS.DOWNLOAD"
          icon="pi pi-download"
          :loading="isDownloading"
          :disabled="isDownloading"
        />
      </div>
    </form>
  </Dialog>
</template>

<script setup>
import { ref, watch } from "vue";
import DatePicker from "primevue/datepicker";
import { APP_CONSTANTS } from "../config/constants";
import { formatFullDate } from "../utils/dateUtils";

const props = defineProps({
  visible: Boolean,
  isDownloading: Boolean,
});

const emit = defineEmits(["update:visible", "download"]);

const buildDefaultForm = () => ({
  attestation_form: "",
  teacher_name: "",
  attestation_date: null,
  semester: "",
  dean_name: "",
});

const formData = ref(buildDefaultForm());

watch(
  () => props.visible,
  (isVisible) => {
    if (isVisible) {
      formData.value = buildDefaultForm();
    }
  },
);

const handleSubmit = () => {
  if (props.isDownloading) return;

  emit("download", {
    attestation_form: formData.value.attestation_form.trim(),
    teacher_name: formData.value.teacher_name.trim(),
    attestation_date: formatFullDate(formData.value.attestation_date),
    semester: formData.value.semester.trim(),
    dean_name: formData.value.dean_name.trim(),
  });
};
</script>
