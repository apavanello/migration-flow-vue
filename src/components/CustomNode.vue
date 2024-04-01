<script setup lang="ts">
import type { NodeProps } from '@vue-flow/core'
import {Handle, Position} from '@vue-flow/core'
import {computed, ref} from "vue";
import {type NodesData, Status} from "./CustomTypes.ts";


interface Props extends NodeProps {
  data: NodesData
}

// props were passed from the slot using `v-bind="customNodeProps"`
const props = defineProps<Props>()
const show = ref(false)
const editShow = ref(false)

const changeOnStatus = computed(() => {
  switch (props.data.status) {
    case Status.STOP:
      return 'gray'
    case Status.RUNNING:
      return 'green'
    case Status.DONE:
      return 'blue'
    case Status.PAUSE:
      return 'red'
  }
})

</script>

<template class="vue-flow__node-custom">
    <Handle id="Right" type="source" :position="Position.Right" />
    <Handle id="Top" type="source" :position="Position.Top" />
    <Handle id="Left" type="source" :position="Position.Left" />
    <Handle id="Bottom" type="target" :position="Position.Bottom" />
  <v-card
      class="mx-1"
      max-width=100%
  >
    <v-card-title class="text-wrap" >
      <v-col>
        {{ props.label }}
      </v-col>
    </v-card-title>
    <v-card-text style="display: flex" class="pa-1">
      <v-container class="py-0">
        <v-row no-gutters>
          <v-col>
            <v-chip
                size="small"
                :color="changeOnStatus"
                variant="flat"
            >
              {{ props.data.status }}
            </v-chip>
          </v-col>
          <v-col>
            <v-chip
                size="small"
                color="primary"
                variant="flat"
                label
                style="text-shadow: -1px 0 black, 0 1px black, 1px 0 black, 0 -1px black;"
            >
              <v-icon icon="mdi-account-multiple" start></v-icon>
              Authorization
            </v-chip>
          </v-col>
        </v-row>
      </v-container>
    </v-card-text>
    <v-card-text class="pa-1">
      <v-container>
        <v-row>
          <v-col class="px-4 py-1">
            <v-chip class="pa-1 ma-0 w-full" label>
              Start: 1:00
            </v-chip>
          </v-col>
          <v-col class="px-4 py-1">
            <v-chip class="pa-1 ma-0 w-full" label>
              End: TBD
            </v-chip>
          </v-col>
        </v-row>
        <v-row>
          <v-col class="px-4 py-1">
            <v-chip class="pa-1 ma-0 w-full" label>
              Planed Start: 1:00
            </v-chip>
          </v-col>
          <v-col class="px-4 py-1">
            <v-chip class="pa-1 ma-0 w-full" label>
              Planed End: 3:00
            </v-chip>
          </v-col>
        </v-row>
      </v-container>
    </v-card-text>
    <v-card-actions>
      <v-btn
          variant="outlined"
          @click="show = !show"
      >
        Detalhes
      </v-btn>
      <v-spacer></v-spacer>
      <v-btn
          variant="outlined"
          @click="editShow = !editShow"
      >
        Edit
      </v-btn>


    </v-card-actions>

    <v-expand-transition>
      <div v-show="show">
        <v-divider></v-divider>
        <v-card-text class="details-text">
          <div>
            Detalhes:
            {{ props.data.description }}
          </div>
          <div>
            Repo:
            <a href="{{ props.data.repo }}">
              {{ props.data.repo }}
            </a>
          </div>
        </v-card-text>
      </div>
    </v-expand-transition>
  </v-card>

</template>
