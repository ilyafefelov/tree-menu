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
    changeType()
    props.model.children.push({ name: 'new stuff' })
}
function removeChild() {
    props.model.children.pop(this.model.children.length - 1)
}
</script>

<template>
    <div class="font-mono ">
        <div class="flex flex-row items-center">
            <div :class="{ bold: isFolder }" @click.capture="toggle" class="flex flex-row items-center">
                {{ model.name }}
                <span v-if="isFolder">[{{ isOpen ? '-' : '+' }}]</span>

            </div>
            <div class="add p-1 bg-blue-100 m-1 hover:bg-blue-200 rounded-xl" @click="addChild">add child</div>
            <div class="add p-1 bg-blue-100 m-1 hover:bg-blue-200 rounded-xl" @click="removeChild">remove child</div>
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