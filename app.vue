<!--
A nested tree component that recursively renders itself.
You can double click on an item to turn it into a folder.
-->

<script setup>

const treeData = ref({
  name: 'My Tree',
  children: [
    { name: 'hello' },
    { name: 'world' },
    {
      name: 'child folder',
      children: [
        {
          name: 'child folder',
          children: [{ name: 'hello' }, { name: 'world' }]
        },
        { name: 'hello' },
        { name: 'world' },
        {
          name: 'child folder',
          children: [{ name: 'hello' }, { name: 'world' }]
        }
      ]
    }
  ]
})

function removeItem() {
  // console.log(arguments)
  console.log("PARENT")
  console.log(arguments)
  // console.log(e)
  // console.log(p)
  // console.log(args)

}

</script>

<template>
  <ul class="p-16">
    <TreeItem class="item" @removeItem="removeItem" :model="treeData"></TreeItem>
  </ul>
  <pre class="bg-blue-100">
    {{ treeData }}
  </pre>
</template>

<style>
.item {
  cursor: pointer;
  line-height: 1.5;
}

.bold {
  font-weight: bold;
}
</style>