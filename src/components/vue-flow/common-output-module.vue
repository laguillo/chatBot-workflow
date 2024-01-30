<script lang="ts" setup>
import { ref } from 'vue'
import { ChevronsUpDownIcon, PlusIcon, AlertCircleIcon, MinusCircleIcon } from 'lucide-vue-next'
import { Tooltip, TooltipContent, TooltipProvider, TooltipTrigger } from '@/components/ui/tooltip'
import { Collapsible, CollapsibleTrigger, CollapsibleContent } from '@/components/ui/collapsible'
import {
  Select,
  SelectContent,
  SelectGroup,
  SelectItem,
  SelectTrigger,
  SelectValue
} from '@/components/ui/select'
import { Input } from '@/components/ui/input'
import {Button} from "@/components/ui/button"

type Output = {
  name: string
  type: string
  description: string
}

const isOpen = ref(true)
const data = ref<Output[]>([])

function handleOnClickAddBtnInInput(e: Event) {
  e.stopPropagation()
  data.value.push({ name: '', type: '', description: '' })
}

function handleClickDeleteBtnInInput(index: number) {
  data.value.splice(index, 1)
}
</script>

<template>
  <div class="rounded-md bg-muted px-3 py-4">
    <Collapsible v-model:open="isOpen">
      <collapsible-trigger as-child>
        <div class="flex w-full items-center justify-between">
          <div class="flex items-center gap-x-2">
            <chevrons-up-down-icon class="h-4 w-4 cursor-pointer" />
            <p>Output</p>
            <tooltip-provider>
              <tooltip>
                <tooltip-trigger>
                  <alert-circle-icon class="h-3 w-3 text-muted-foreground" />
                </tooltip-trigger>
                <tooltip-content>
                  <p class="max-w-60">
                    Enter the information that needs to be added to the prompt words, which can be referenced by the
                    prompt words.
                  </p>
                </tooltip-content>
              </tooltip>
            </tooltip-provider>
          </div>
          <Button variant="ghost">
            <plus-icon class="h-4 w-4 cursor-pointer text-primary" @click="handleOnClickAddBtnInInput" />
          </Button>
        </div>
      </collapsible-trigger>
      <collapsible-content class="space-y-3 px-3 py-3">
        <div class="flex gap-x-4">
          <Label class="w-4/12 text-sm text-muted-foreground">Name</Label>
          <Label class="w-2/12 text-sm text-muted-foreground">Type</Label>
          <Label class="w-6/12 text-sm text-muted-foreground">Description</Label>
        </div>
        <div class="flex gap-x-4" v-for="(item, index) in data" :key="index">
          <div class="w-3/12">
            <Input v-model="item.name" placeholder="Enter name" />
          </div>
          <div class="w-3/12">
            <Select>
              <SelectTrigger class="w-full">
                <SelectValue placeholder="Select a fruit" />
              </SelectTrigger>
              <SelectContent>
                <SelectGroup>
                  <SelectItem value="String"> String </SelectItem>
                  <SelectItem value="Integer"> Integer </SelectItem>
                  <SelectItem value="Boolean"> Boolean </SelectItem>
                  <SelectItem value="Number"> Number </SelectItem>
                  <SelectItem value="Object" disabled> Object </SelectItem>
                  <SelectItem value="Array<String>"> {{'Array<String>'}} </SelectItem>
                  <SelectItem value="Array<Integer>"> {{'Array<Integer>'}}  </SelectItem>
                  <SelectItem value="Array<Boolean>">  {{'Array<Boolean>'}}  </SelectItem>
                  <SelectItem value="Array<Number>">  {{'Array<Number>'}} </SelectItem>
                  <SelectItem value="Array<Object>" disabled>  {{'Array<Object>'}}  </SelectItem>
                </SelectGroup>
              </SelectContent>
            </Select>
          </div>
          <div class="flex w-6/12 cursor-pointer items-center gap-x-2 text-primary">
            <Input v-model="item.description" placeholder="Describe what the variable is used for" />
            <minus-circle-icon class="h-4 w-4" @click="() => handleClickDeleteBtnInInput(index)" />
          </div>
        </div>
      </collapsible-content>
    </Collapsible>
  </div>
</template>