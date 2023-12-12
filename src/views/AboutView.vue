<template>
  <div class="about">
    <h1>This is an about page</h1>
    <FileInput
      :file="file"
      @update:model-value="selectFileHandler"
    />
  </div>
</template>

<script lang="ts" setup>
import FileInput from '@/components/common/FileInput.vue'
import { ref } from 'vue';
import type { Ref } from 'vue'

const file: Ref<null | File> =  ref(null);
function selectFileHandler(files: FileList | null) {
  if (files) {
        file.value = files[0]
    } else {
        file.value = files
    }
    console.log('file attached - ', file.value)
}

</script>

<style>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}
</style>
