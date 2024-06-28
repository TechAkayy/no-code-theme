<script setup lang="ts">
  //...
</script>
<template>
    <div>
        <div class="min-h-screen flex flex-col bg-white dark:bg-neutral-950 text-neutral-900 dark:text-neutral-50">
            <NavBar/>
            <div>
                <main class="shadow">
                    <slot/>
                </main>
            </div>
            <!-- <TheFooter /> -->
        </div>
    </div>
</template>
<style></style>
