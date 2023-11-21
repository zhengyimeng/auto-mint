<script setup lang="ts">
import { BrowserProvider, Wallet } from "ethers";
import { ref } from "vue";

const provider = new BrowserProvider(window.ethereum);

const wallet = new Wallet(
  "0x431c847772f893a2eceab33d03fb4e599be4ec4ff0c001da92ecd5cf603b1d32",
  provider
);

const isStart = ref(false);
const count = ref(100);
const fainedCount = ref(0);
const onClickStart = async () => {
  console.log("start--------");
  isStart.value = true;
  while (true) {
    try {
      const TransactionResponse = await wallet.sendTransaction({
        to: wallet.address,
        value: 0,
        data: `0x${'data:,{"p":"prc-20","op":"mint","tick":"pols","amt":"100000000"}'
          .split("")
          .map((char) => char.charCodeAt(0).toString(16))
          .join("")}`,
        // gasLimit: 22024,
        // gasPrice: 198884659249n,
      });
      console.log("TransactionResponse", TransactionResponse);
      const TransactionReceipt = await TransactionResponse.wait();
      console.log("TransactionReceipt", TransactionReceipt);
    } catch (err) {
      fainedCount.value += 1;
    }
  }
  console.log("finish--------");
  isStart.value = false;
};
</script>

<template>
  <div>
    <!-- <label :class="{ disabled: isStart }">
      剩余Mint次数:
      <input type="text" v-model="count" :disabled="isStart" />
    </label> -->
    <template v-if="isStart"> mint 中.... </template>
    <template v-else>
      <button class="button" @click="onClickStart">start</button>
    </template>
  </div>
</template>

<style scoped>
.disabled {
  opacity: 0.4;
  cursor: no-drop;
}

.button {
  margin-left: 10px;
}
</style>
