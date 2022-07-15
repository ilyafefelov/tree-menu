<script setup>


const props = defineProps({
    model: Object
})




const description = ref()

const isOpen = ref(false)
const isFolder = computed(() => {
    return props.model.children && props.model.children.length
})


function addDesc() {
    console.log(description.value)
    props.model.description = description.value
    description.value = ''

}
function toggle() {
    isOpen.value = !isOpen.value
}

function changeType() {
    if (!isFolder.value) {
        props.model.children = []
        isOpen.value = true
    }
}

function addChild() {
    changeType()
    let id = `${props.model.id}-${props.model.children.length + 1}`
    props.model.children.unshift({ name: 'new stuff', id })
    isOpen.value = true
}
function removeChild(e) {
    emit('removeItem', e, props.model)
}

function removeIt(e, p) {
    emit('removeItem', e, p)
}

const emit = defineEmits(['removeT', 'removeItem', 'removeThis'])

</script>

<template>
    <div class="font-mono ">
        <div class="flex flex-row items-center border w-fit">
            <div :class="{ bold: isFolder }" @click.capture="toggle" class="flex flex-row items-center">
                {{ model.name }}
                <span v-if="isFolder">[{{ isOpen ? '-' : '+' }}]</span>

            </div>
            <div class="add p-1 bg-blue-100 m-1 hover:bg-blue-200 rounded-xl" @click="addChild">add child</div>
            <div class="add p-1 bg-blue-100 m-1 hover:bg-blue-200 rounded-xl" @click="removeChild($event)">
                remove child</div>
            <input v-model="description" placeholder="add descrption">
            <p>{{ description }}</p>
            <button class="bg-yellow-100 border m-2 p-1" @click="addDesc">add desc</button>
            <div>{{ model.description }}</div>
        </div>
        <ul v-show="isOpen" v-if="isFolder">

            <TreeItem class="item ml-6" v-for="model in model.children" @removeItem="removeIt" :model="model">
            </TreeItem>

        </ul>
    </div>
</template>