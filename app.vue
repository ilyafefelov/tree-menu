<!--
A nested tree component that recursively renders itself.
You can double click on an item to turn it into a folder.
-->

<script setup>

const treeData = ref({
  name: 'My Tree',
  children: [
    { name: 'hello', id: '1' },
    { name: 'world', id: '2' },
    {
      name: 'child folder',
      id: '3',
      children: [
        {
          name: 'child folder',
          id: '3-1',
          children: [{ name: 'hello', id: '3-1-1' }, { name: 'world', id: '3-1-2' }]
        },
        { name: 'hello', id: '3-2' },
        { name: 'world', id: '3-3' },
        {
          name: 'child folder', id: '3-4',
          children: [{ name: 'hello', id: '3-4-1', }, { name: 'world', id: '3-4-2', }]
        }
      ]
    }
  ]
})

function removeItem() {
  // console.log(arguments)
  console.log("PARENT")
  console.log(arguments[1])
  // props.model.children.splice(props.model.children.findIndex(({ name }) => name == props.model.name), 1);
  // console.log(Object.values(treeData.value))
  // treeData.value.findIndex((n) => n == arguments[1].name)
  // console.log(e)
  // console.log(p)
  // console.log(args)
  // console.log("found:", findNested(treeData.value, 'name', arguments[1].name))
  console.log("found:", findKey(treeData.value, arguments[1].name))




}

// function findNested(obj, key, value) {
//   // Base case
//   if (obj[key] === value) {
//     return obj;
//   } else {
//     // Recursive case
//     // console.log(obj['children'])
//     if (obj['children']) {
//       // console.log(obj)
//       for (let i = 0; i < obj['children'].length; i++) {
//         if (obj['children'][i][key] === value) {
//           return obj['children'][i];
//         }
//       }
//     }
//   }
// }

function findKey(obj, target) {
  const fnd = obj => {
    for (const [k, v] of Object.entries(obj)) {
      if (v === target) return [k, v, Object.entries(obj)];
      if (typeof v === 'object') {
        const f = fnd(v);
        if (f) return f;
      }
    }
  }
  return fnd(obj);
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