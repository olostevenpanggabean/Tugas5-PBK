<script setup>
import { ref, computed, watch, onMounted} from 'vue';

const newItem = ref('');
const shoppingList = ref([
  { name: 'Susu', bought: false },
  { name: 'Beras', bought: true },
  { name: 'Telur', bought: false },
]);

function addItem() {
  shoppingList.value.push({ name: newItem.value || 'Item kosong', bought: false });
  newItem.value = '';
}


function toggleBought(index) {
  shoppingList.value[index].bought = !shoppingList.value[index].bought;
}

const remainingItems = computed(() => {
  return shoppingList.value.filter((item) => !item.bought);
});

watch(
  () => [...shoppingList.value],
  (newValue, oldValue) => {
    if (newValue.length > oldValue.length) {
      console.log('Item baru ditambahkan:', newValue[newValue.length - 1]);
    }
  },
  { deep: true }
);

const inputRef = ref(null);
const addButtonRef = ref(null);

onMounted(() => {
    console.log('Komponen telah dimont.');
    if (inputRef.value) {
        inputRef.value.focus();
    }

    if (addButtonRef.value) {
        addButtonRef.value.disabled = !newItem.value.trim();
    }
})
</script>

<template>
  <div>
    <input
      ref="inputRef"
      v-model="newItem"
      placeholder="Tambahkan item..."
      @input="addButtonRef.disabled = !newItem.trim()"
    />
    <button ref="addButtonRef" @click="addItem">
      Tambah
    </button>

    <h3>Semua Item:</h3>
    <ul>
      <li v-for="(item, index) in shoppingList" :key="index">
        <span :class="{ bought: item.bought }">{{ item.name }}</span>
        <button @click="toggleBought(index)">
          {{ item.bought ? 'Batal' : 'Beli' }}
        </button>
      </li>
    </ul>

    <h3>Belum Dibeli:</h3>
    <ul>
      <li v-for="(item, index) in remainingItems" :key="index">
        {{ item.name }}
        <button @click="toggleBought(shoppingList.indexOf(item))">
          Tandai sebagai Dibeli
        </button>
      </li>
    </ul>
  </div>
</template>
