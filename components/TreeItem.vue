<script setup>
import { ref, computed } from 'vue'

const props = defineProps({
    model: Object
})

const isOpen = ref(false)
const isFolder = computed(() => {
    return props.model.children && props.model.children.length
})

function toggle() {
    isOpen.value = !isOpen.value
}

function changeType() {
    if (!isFolder.value) {
        props.model.children = []
        addChild()
        isOpen.value = true
    }
}

function addChild() {
    props.model.children.push({ name: 'new stuff' })
}
</script>

<template>
    <div class="bg-red-200 font-mono">
        <div :class="{ bold: isFolder }" @click="toggle" @dblclick="changeType">
            {{ model.name }}<div class="add" @click="addChild">/add child/</div>
            <span v-if="isFolder">[{{ isOpen ? '-' : '+' }}]</span>

        </div>
        <ul v-show="isOpen" v-if="isFolder">
            <!--
        A component can recursively render itself using its
        "name" option (inferred from filename if using SFC)
      -->
            <TreeItem class="item ml-6" v-for="model in model.children" :model="model">
            </TreeItem>

        </ul>
    </div>
</template>