<template>
    <div>
        <SectionsHero/>
        <SectionsFirstContainer/>
        <SectionsSecondContainer/>
        <SectionsThirdContainer/>
        <SectionsFourthContainer/>
        <SectionsFifthContainer/>
    </div>
</template>

<script setup lang="ts">
import LocomotiveScroll from 'locomotive-scroll'

onMounted(() => {
    new LocomotiveScroll()
})
</script>
