<script setup>
const props = defineProps({
    model: Object,
    modelname: String,
});

const m = props.model.name

const description = ref();
const showDelete = ref(false);

const isOpen = ref(false);
const isFolder = computed(() => {
    return props.model.children && props.model.children.length;
});

function addDesc() {
    console.log(description.value);
    props.model.description = description.value;
    description.value = "";
}
function toggle() {
    isOpen.value = !isOpen.value;
}

function changeType() {
    if (!isFolder.value) {
        props.model.children = [];
        isOpen.value = true;
    }
}

function addChild(e) {
    changeType();
    let id = `${props.model.id}-${props.model.children.length + 1}`;
    props.model.children.unshift({ name: "new stuff", id });
    isOpen.value = true;
    // console.log(e)
    // emit("aler", props.model, props.model.children[0])
    emit("alz", props.model)
    // alert(
    //     `here: ${props.model.name}
    //     new: ${props.model.children[0].name}
    //     model: ${m}
    //     `
    // )
}

function alb(e) {
    // console.log("e", e)
    emit('alz', e)
}

function askToRemove() {
    showDelete.value = !showDelete.value;
}


function removeChild(e) {
    emit("removeItem", e, props.model);
}

function removeIt(e, p) {
    emit("removeItem", e, p);
}

const emit = defineEmits(["alz", "aler", "removeT", "removeItem", "removeThis"]);
</script>

<template>
    <div class="font-mono">
        <div class="flex bg-white flex-row items-center border w-fit pl-2">
            <div :class="{ bold: isFolder }" @click.capture="toggle" class="flex flex-row items-center">
                {{ model.name }}
                <span v-if="isFolder">[{{ isOpen ? "-" : "+" }}]</span>
            </div>
            <div class="add p-1 ml-3 bg-blue-100 m-1 hover:bg-blue-200 rounded-xl" v-if="model.name !== 'My Tree'"
                @click="addChild($event)">
                add child
            </div>
            <div v-if="model.name !== 'My Tree'" class="p-1 bg-red-300 m-1 hover:bg-blue-200 rounded-xl"
                @click="askToRemove">
                delete ?
            </div>
            <div v-if="model.name !== 'My Tree'" class="p-1 bg-red-300 m-1 hover:bg-blue-200 rounded-xl"
                @click="removeChild($event)">
                remove child
            </div>
            <input class="px-2 border" v-model="description" placeholder="add descrption" />
            <p>{{ description }}</p>
            <button class="bg-yellow-100 border m-2 p-1" @click="addDesc">
                add desc
            </button>
            <div>{{ model.description }}</div>
        </div>
        <ul v-show="isOpen" v-if="isFolder">
            <!-- {{ slotProps.text }} -->
            <slot :text="modelname"></slot>
            <div id="parentName">
                {{ model.name }}
            </div>
            <TreeItem v-slot="slotProps" class="item ml-6" @alz="alb($event, model)" v-for="model in model.children"
                key="model.id" @removeItem="removeIt" :model="model" :modelname="model.name">

            </TreeItem>
        </ul>
    </div>
</template>
