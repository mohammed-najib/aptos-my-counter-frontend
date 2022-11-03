<script setup lang="ts">
import { AptosClient } from "aptos";
const moveModule = import.meta.env.VITE_MOVE_MODULE_ADDRESS
const client = new AptosClient("https://fullnode.devnet.aptoslabs.com/v1");

console.log(await client.getAccountResources(moveModule));
</script>

<template>
  <div>Some Text</div>
</template>
