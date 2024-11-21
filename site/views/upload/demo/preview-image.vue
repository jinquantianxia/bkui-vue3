<template>
  <div class="demo">
    <bk-upload
      :files="files"
      :handle-res-code="handleRes"
      :is-show-preview="true"
      :multiple="false"
      :url="'https://jsonplaceholder.typicode.com/posts/'"
      theme="picture"
      with-credentials
      @delete="handleDelete"
      @preview="handlePreview"
    />

    <bk-dialog
      v-model:is-show="previewVisible"
      :close-icon="false"
      :title="previewTitle"
      quick-close
    >
      <img
        width="100%"
        :src="previewImage"
      />
    </bk-dialog>
  </div>
</template>

<script setup>
  import { ref } from 'vue';

  import BkUpload from '@bkui-vue/upload';

  import babyqImgUrl from '../../../imgs/babyq.png';

  const files = ref([
    {
      url: babyqImgUrl,
    },
  ]);
  const previewTitle = ref('图片预览');
  const previewImage = ref('');
  const previewVisible = ref(false);

  const handleDelete = (file, fileList) => {
    console.log(file, fileList, 'handleDelete');
  };

  const handleRes = response => {
    if (response.id) {
      return true;
    }
    return false;
  };

  const handlePreview = async file => {
    if (!file.url) {
      file.url = await getBase64(file);
    }
    previewImage.value = file.url;
    previewVisible.value = true;
    previewTitle.value = file.name || file.url.substring(file.url.lastIndexOf('/') + 1);
  };

  const getBase64 = file => {
    return new Promise((resolve, reject) => {
      const reader = new FileReader();
      reader.readAsDataURL(file);
      reader.onload = () => resolve(reader.result);
      reader.onerror = error => reject(error);
    });
  };
</script>

<style scoped>
  :deep(.bk-modal-footer) {
    display: none;
  }
</style>
