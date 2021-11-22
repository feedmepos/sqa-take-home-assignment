<template>
  <div id="bot-control-panel">
    <h4>McDonald Cooking Bot Control Panel</h4>
    <div class="button-group">
      <button @click="addBot" id="add-bot-btn">Add Bot</button>
      <button @click="removeBot" id="remove-bot-btn">Remove Bot</button>
    </div>
    <div id="bot-section">
      <div v-for="b in bots" :key="b.id" class="bot">
        Bot {{ b.id }} {{ b.orderId ? "Processing order " + b.orderId : "" }}
      </div>
    </div>
  </div>
  <div>
    <h4>McDonald Customer Display</h4>
    <div class="button-group">
      <button @click="addOrder" id="add-order-btn">Add Order</button>
      <button @click="addVIPOrder" id="add-vip-order-btn">Add VIP Order</button>
    </div>
    <div id="customer-display-zone">
      <div id="pending-container">
        <h5>Pending</h5>
        <div v-for="o in pendingOrders" :key="o.id" class="order">
          Order {{ o.id }} {{ o.isVIP ? "VIP" : "" }}
          {{ o.botId ? "processing by Bot " + o.botId : "" }}
        </div>
      </div>
      <div id="complete-container">
        <h5>Complete</h5>
        <div v-for="o in completedOrders" :key="o.id" class="order">
          Order {{ o.id }} {{ o.isVIP ? "VIP" : "" }}
          {{ o.botId ? "completed by Bot " + o.botId : "" }}
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { computed, defineComponent, reactive } from "vue";

interface Order {
  id: number;
  isVIP: boolean;
  botId: number | null;
  completed: boolean;
}

interface Bot {
  id: number;
  orderId: number | null;
}

function sortOrderFn(a: Order, b: Order) {
  if (a.isVIP != b.isVIP) {
    return a.isVIP ? -1 : 1;
  }
  return a.id - b.id;
}

export default defineComponent({
  name: "App",
  components: {},
  setup() {
    let botIndex = 1;
    const orders = reactive<{ list: Order[] }>({ list: [] });
    const pendingOrders = computed(() =>
      orders.list.filter((o) => !o.completed).sort(sortOrderFn)
    );
    const bots = reactive<{ list: Bot[] }>({ list: [] });
    const addBot = () => {
      const bot = { id: botIndex++, orderId: null, timeout: null };
      bots.list.push(bot);
      botGetTask(bot);
    };
    const removeBot = () => {
      var bot = bots.list.pop();
      if (bot?.orderId) {
        var order = pendingOrders.value.find((o) => o.id === bot?.orderId);
        if (order) {
          order.botId = null;
          bot.orderId = null;
        }
      }
    };

    let orderIndex = 1;
    const addOrder = (isVip = false) => {
      orders.list.push({
        id: orderIndex++,
        isVIP: isVip,
        botId: null,
        completed: false,
      });
      const freeBot = bots.list.filter((b) => b.orderId === null)[0];
      if (freeBot) {
        botGetTask(freeBot);
      }
    };

    function botGetTask(bot: Bot) {
      const freePendingOrder = pendingOrders.value.filter(
        (o) => o.botId === null
      )[0];
      if (freePendingOrder) {
        freePendingOrder.botId = bot.id;
        bot.orderId = freePendingOrder.id;
        setTimeout(() => {
          freePendingOrder.completed = true;
          bot.orderId = null;
          botGetTask(bot);
        }, 3000);
      }
    }

    return {
      bots: computed(() => bots.list),
      addBot,
      removeBot,
      addOrder: () => addOrder(),
      addVIPOrder: () => addOrder(true),
      pendingOrders: pendingOrders,
      completedOrders: computed(() =>
        orders.list.filter((o) => o.completed).sort(sortOrderFn)
      ),
    };
  },
});
</script>

<style>
#bot-section {
  display: grid-column;
}

.button-group {
  padding: 10px 0px;
}

.bot {
  background-color: rgb(192, 192, 192); /* Green */
  padding: 10px 30px;
  margin: 3px 3px;
  text-align: center;
}

.order {
  background-color: rgb(176, 170, 255); /* Green */
  padding: 10px 30px;
  margin: 3px 3px;
  text-align: center;
}

button {
  background-color: #abc2f3; /* Green */
  border: none;
  color: white;
  padding: 15px 32px;
  margin: 0px 3px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
}

#pending-container {
  background: rgb(255, 243, 224);
}

#complete-container {
  background: rgb(224, 255, 224);
}

#customer-display-zone {
  display: grid;
  grid-template-columns: 1fr 1fr;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 30px;
  display: grid;
  gap: 10px;
  grid-template-columns: 1fr 3fr;
}
</style>
