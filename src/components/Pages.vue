<script setup lang="ts">
import {
  Pagination,
  PaginationEllipsis,
  PaginationFirst,
  PaginationLast,
  PaginationList,
  PaginationListItem,
  PaginationNext,
  PaginationPrev,
} from "./ui/pagination";

import { Button } from "./ui/button";
import { defineProps, defineEmits, ref } from "vue";
defineProps<{
  totalPages: number;
}>();

const pageNo = ref(1);

const emit = defineEmits(["page-changed"]);

function changePage(newPage: number) {
  if (newPage !== pageNo.value) {
    pageNo.value = newPage;
    emit("page-changed", newPage);
  }
}
</script>

<template>
  <Pagination
    v-slot="{ page }"
    :total="totalPages * 10"
    :sibling-count="1"
    show-edges
    :default-page="1"
  >
    <PaginationList v-slot="{ items }" class="flex items-center gap-1">
      <PaginationFirst @click="changePage(1)" />
      <PaginationPrev @click="changePage(Math.max(1, pageNo - 1))" />
      <template v-for="(item, index) in items">
        <PaginationListItem v-if="item.type === 'page'" :key="index" :value="item.value" as-child>
          <Button
            @click="changePage(item.value)"
            class="w-10 h-10 p-0"
            :variant="item.value === page ? 'default' : 'outline'"
          >
            {{ item.value }}
          </Button>
        </PaginationListItem>
        <PaginationEllipsis v-else :key="item.type" :index="index" />
      </template>

      <PaginationNext @click="changePage(Math.min(totalPages, pageNo + 1))" />
      <PaginationLast @click="changePage(totalPages)" />
    </PaginationList>
  </Pagination>
</template>
