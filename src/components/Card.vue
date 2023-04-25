<script setup>
const props = defineProps(["type", "headline", "subHeadline", "text", "link"]);
</script>

<template>
  <div class="card flex-flow" :class="props.type">
    <h3>{{ props.headline }}</h3>
    <h4>{{ props.subHeadline }}</h4>
    <p>
      {{ props.text }}
    </p>
    <a :href="props.link.url">{{ props.link.label }}</a>
  </div>
</template>
