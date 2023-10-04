<template>
  <div class="main__container container">
    <my-action></my-action>
    <my-editor></my-editor>
  </div>
</template>

<script>
import MyAction from '@/components/MyAction';
import MyEditor from '@/components/MyEditor';
export default {
  components: {
    MyAction,
    MyEditor,
  }

}

</script>
