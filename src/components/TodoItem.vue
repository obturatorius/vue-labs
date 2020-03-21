<template>
  <li>{{ todo.text }}</li>
</template>

<script lang="ts">
import Vue from "vue";
import Component from "vue-class-component";
import { Prop } from "vue-property-decorator";

export interface Todo {
  id: number;
  text: string;
}

@Component
export default class TodoItem extends Vue {
  @Prop() private todo!: Todo;
}
</script>
