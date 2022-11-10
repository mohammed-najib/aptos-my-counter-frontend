<script setup lang="ts">
import { AptosClient } from "aptos";
import { onBeforeMount, ref } from "vue";
const moveModule = import.meta.env.VITE_MOVE_MODULE_ADDRESS;
const client = new AptosClient("https://fullnode.devnet.aptoslabs.com/v1");

const count = ref(0);
const walletAddress = ref("");

const getCount = async () => {
  const resources = await client.getAccountResources(walletAddress.value);
  const resourceType = `${moveModule}::counter::CounterHolder`;
  const resource = resources.find((e) => e.type === resourceType);
  const count = (resource?.data as any).count;

  return count;
};

const bump = async () => {
  const transaction = {
    type: "entry_function_payload",
    function: `${moveModule}::counter::bump`,
    arguments: [],
    type_arguments: [],
  };

  const txHash = await (window as any).aptos.signAndSubmitTransaction(
    transaction
  );

  console.log("txHash is: ", txHash);

  count.value = await getCount();
};

onBeforeMount(async () => {
  const wallet = await (window as any).aptos.connect();
  walletAddress.value = wallet.address;
  count.value = await getCount();
});
</script>

<template>
  <div>
    <p>Your count: {{ count }}</p>
    <button @click="bump()">bump</button>
  </div>
</template>
