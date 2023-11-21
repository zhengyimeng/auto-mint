<script setup lang="ts">
import { BrowserProvider, Wallet } from "ethers";
import { ref } from "vue";

const provider = new BrowserProvider(window.ethereum);

const walletPrivate = ref("");
const sendData = ref(
  'data:,{"p":"prc-20","op":"mint","tick":"pols","amt":"100000000"}'
);

const isStart = ref(false);
const fainedCount = ref(0);
const onClickStart = async () => {
  console.log("start--------");
  isStart.value = true;
  if (!walletPrivate.value) return;
  const wallet = new Wallet(walletPrivate.value, provider);
  while (true) {
    try {
      const TransactionResponse = await wallet.sendTransaction({
        to: wallet.address,
        value: 0,
        data: `0x${sendData.value
          .split("")
          .map((char) => char.charCodeAt(0).toString(16))
          .join("")}`,
        // gasPrice: 198884659249n,
      });
      console.log("TransactionResponse", TransactionResponse);
      const TransactionReceipt = await TransactionResponse.wait();
      console.log("TransactionReceipt", TransactionReceipt);
    } catch (err) {
      fainedCount.value += 1;
    }
  }
};
</script>

<template>
  <div class="box">
    <label for=""
      >Wallet私钥：
      <input type="text" v-model="walletPrivate" />
    </label>
    <label for=""
      >写入数据：
      <input type="text" v-model="sendData" />
    </label>
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

input {
  width: 800px;
  height: 30px;
  font-size: 18px;
}

.button {
  width: 200px;
  height: 50px;
  margin-left: 10px;
}

.box {
  display: flex;
  flex-direction: column;
  gap: 10px;
}
</style>
