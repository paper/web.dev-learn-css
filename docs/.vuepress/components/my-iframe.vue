<template>
  <div>
    <iframe style="width: 100%; height: 400px; border: 0 none;" :src="src + '?height=400&theme-id=light&default-tab=result&editable=true'"></iframe>
    <p>
      <a target="_blank" :href="src">{{src}}</a>
    </p>
  </div>
</template>
<script>
export default {
  props: ['src'],
  data() {
    return {
      
    }
  },
  mounted () {
    
  }
}
</script>