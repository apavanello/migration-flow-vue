<script lang="ts" setup>
import {getCurrentInstance, ref} from "vue"
import {ContextMenu, ContextMenuGroup, ContextMenuItem } from "@imengyu/vue3-context-menu"
import {getNodes} from "./Nodes.ts"
import {getEdges} from "./Edges.ts"
import {Status} from "./CustomTypes.ts"
import axios from "axios"

// import Details from "./Details.vue";
import {type Node, type NodeMouseEvent} from "@vue-flow/core"

const show = ref(false);
const node = ref({} as Node);
// const details = Details


const options = ref({
  zIndex: 3,
  minWidth: 230,
  x: 500,
  y: 200
});

defineExpose({
  ShowNodeContextMenu
})

function ShowNodeContextMenu (e : NodeMouseEvent) {

    e.event.preventDefault();
  if ("x" in e.event) {
    options.value.x = e.event.x;
  }
  if ("y" in e.event) {
    options.value.y = e.event.y;
  }
    node.value = e.node;
    show.value = true;

}

async function deleteNode(node: Node) {
    await axios.post(
        '/api/node/delete',
        {
            id: node.id
        },
        {
            headers: {
                'Content-Type': 'application/json',
                'Delete-Type': 'node'
            }
        }
    );
    console.log('deleteNode')
    refresh();
}

async function updateNodeStatus(node: Node, status: Status) {
    await axios.post(
        '/api/node/update',
        {
            id: node.id,
            status: status
        },
        {
            headers: {
                'Content-Type': 'application/json',
                'Update-Type': 'status'
            }
        }
    );
    console.log('updateNodeStatus')
    refresh();
}
function refresh() {

    getNodes()
    getEdges()
    const instance = getCurrentInstance();
    instance?.proxy?.$forceUpdate();

    console.log('refresh')
}
</script>

<template>
  <context-menu
      v-model:show="show"
      :options="options"
  >
    <context-menu-group label="Alterar Status">
      <context-menu-item label="Não Iniciado" @click="updateNodeStatus(node, Status.STOP)"/>
      <context-menu-item label="Em execução" @click="updateNodeStatus(node, Status.RUNNING)"/>
      <context-menu-item label="Pausado" @click="updateNodeStatus(node, Status.PAUSE)"/>
      <context-menu-item label="Concluído" @click="updateNodeStatus(node, Status.DONE)"/>
  </context-menu-group>
    <context-menu-sperator/>
    <context-menu-item label="Excluir Node" @click="deleteNode(node)"/>
  </context-menu>
</template>