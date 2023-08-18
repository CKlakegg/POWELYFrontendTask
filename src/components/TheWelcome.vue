<template>
  <div>
    OTHER CAPEX AND OPEX COSTS
    <form @submit.prevent="submitForm" class="form">
      <div >
        <label for="costName">New Cost Name</label>
        <input v-model="newCostName" type="text" id="costName" class="input" placeholder="Enter Cost Name" required>
      </div>
      <div >
        <label for="category">Category</label>
        <select v-model="selectedCategory" id="category" class="input">
          <option value="category1">Category 1</option>
          <option value="category2">Category 2</option>
        </select>
      </div>
      <div >
        <label for="costValue">Cost</label>
        <input v-model="newCostValue" type="text" id="costValue" class="input" placeholder="Cost" required>
      </div>
      <div >
        <label for="frequency" class="text">Frequency</label>
        <select v-model="selectedFrequency" id="frequency" class="input">
          <option value="EUR">EUR</option>
          <option value="USD">USD</option>
        </select>
      </div>
      <button type="submit" class="add-button">{{ isEditing ? 'Update' : 'Add' }}</button>
    </form>

    <h4>FIXED COSTS (CAPEX)</h4>
    <div class="backgroundCart">
     
      <ul>
        <li v-for="(cost, index) in costList" :key="index">
          <span>{{ cost.name }}</span>
          <span>{{ cost.value }} {{ cost.currency }}</span>
        </li>
      </ul>
    </div>
  </div>
</template>

<script setup>
import axios from 'axios'
import { ref, computed } from 'vue'

const newCostName = ref('');
const newCostValue = ref('');
const selectedCategory = ref('category1');
const selectedFrequency = ref('EUR');
const costList = ref([]);

const editingIndex = ref(-1);

const isEditing = computed(() => editingIndex.value !== -1);

const submitForm = async () => {
  if (newCostName.value.trim() === '' || newCostValue.value.trim() === '') {
    return;
  }

  if (isEditing) {
    costList.value[editingIndex.value] = {
      name: newCostName.value,
      value: newCostValue.value,
      category: selectedCategory.value,
      currency: selectedFrequency.value,
    };
    editingIndex.value = -1;
  } else {
    const postData = {
      name: newCostName.value,
      value: newCostValue.value,
      category: selectedCategory.value,
      currency: selectedFrequency.value,
    };

    costList.value.push(postData);
  }

  newCostName.value = '';
  newCostValue.value = '';
}

const editCost = (index) => {
  editingIndex.value = index;
  newCostName.value = costList.value[index].name;
  newCostValue.value = costList.value[index].value;
  selectedCategory.value = costList.value[index].category;
  selectedFrequency.value = costList.value[index].currency;
}
</script>

<style scoped>
.text h3 {
  text-align: center;
  color: black;
}

.form {
  background-color: var(--vt-c-white-soft);
  padding: 30px;
  display: flex;
}

.background {
  color: var(--background-form);
}

.backgroundCart {
  margin-top: 20px;
  padding-top: 20px;
  background-color: white;
}
</style>
