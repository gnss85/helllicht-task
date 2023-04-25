<script setup>
import { Card } from "./";

// backend simulation
import services from "../assets/services.json";
</script>

<template>
  <section aria-labelledby="services--heading" class="services">
    <h2 id="services--heading">Leistungen</h2>
    <div class="card-wrapper grid-flow">
      <Card
        v-for="service in services"
        :key="service.index"
        :type="service.type"
        :headline="service.headline"
        :subHeadline="service.subHeadline"
        :text="service.text"
        :link="service.link"
      />
    </div>
  </section>
</template>
