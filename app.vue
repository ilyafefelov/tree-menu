<script setup>
const treeData = ref({
  name: "My Tree",
  id: "root",
  children: [
    { name: "hello", id: "1" },
    { name: "world", id: "2" },
    {
      name: "child folder",
      id: "3",
      children: [
        {
          name: "child folder",
          id: "3-1",
          children: [
            { name: "hello", id: "3-1-1" },
            { name: "world", id: "3-1-2" },
          ],
        },
        { name: "hello", id: "3-2" },
        { name: "world", id: "3-3" },
        {
          name: "child folder",
          id: "3-4",
          children: [
            { name: "hello", id: "3-4-1" },
            { name: "world", id: "3-4-2" },
          ],
        },
      ],
    },
  ],
});

function als(e, p) {
  // console.log(e);


  let value = e.id.split('-');

  if (value.length > 1) {
    value.pop()
    value = value.join('-')
  } else {
    value = 'root'
  }

  // console.log("parent is:", value);

  let parent = findParent(treeData.value, "id", value)

  console.log("parent", parent)
  console.log("middle", e)
  console.log("child", e.children[0])
  alert(`Parent:${parent.name} \nMiddle: ${e.name} \n newChild: ${e.children[0].name}`);

}
function findParent(obj, key, value) {
  if (obj.id == value) {
    // console.log("foundPArent:", obj);
    return obj;
  }
  if (obj.children) {
    for (let i = 0; i < obj.children.length; i++) {
      if (obj.children[i].id == value) {
        // console.log("foundPArent:", obj.children[i]);
        return obj.children[i];
      }
      findParent(obj.children[i], key, value);
    }
  }
}

function removeItem() {
  findKey(treeData.value, "id", arguments[1].id);
}
function findKey(obj, key, value) {
  // console.log(obj)

  if (obj.id == value) {
    return obj;
  }
  if (obj.children) {
    for (let i = 0; i < obj.children.length; i++) {
      if (obj.children[i].id == value) {
        console.log(obj.children[i]);
        console.log(i);
        obj.children.splice(i, 1);
        return obj.children[i];
      }
      findKey(obj.children[i], key, value);
    }
  }
}
</script>

<template>
  <div class="pt-10">
    <nuxt-link target="_blank" class="bg-blue-100 rounded w-fit hover:bg-blue-200 p-2 ml-16 border"
      to="https://github.com/ilyafefelov/tree-menu">
      CODE
    </nuxt-link>
    <ul class="p-16 bg-green-200">
      <TreeItem v-slot="slotProps" class="item" @alz="als" @removeItem="removeItem" :model="treeData"
        :modelname="treeData.name">
      </TreeItem>
    </ul>
    <pre class="bg-blue-100">
      {{ treeData }}
    </pre>
  </div>
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
