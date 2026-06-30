<script setup>
  import { ref } from 'vue'

  const showForm = ref(false);
  const newMemo = ref("");
  const memos = ref([]);
  const errorMsg = ref("");

  function addMemo() {
    if (!newMemo.value) {
      errorMsg.value = "Please enter a memo";
      return;
    }
    memos.value.push({
      id: Date.now(),
      content: newMemo.value,
      date: new Date().toLocaleString().split(",")[0],
      backgrounColor: getRandomColor()
    });
    showForm.value = false;
    newMemo.value = "";
  }

  function getRandomColor() {
    return `#${Math.floor(Math.random() * 16777215).toString(16)}`
  }

  function deleteMemo(id) {
    memos.value = memos.value.filter(memo => memo.id !== id)
  }
</script>

<template>
  <main>
    <div class="container">
      <header>
        <h1 class="header-title">Memo</h1>
        <button class="header-button" @click="showForm = true">+</button>
      </header>
      <div class="card-container">
        <div class="card" v-for="memo in memos" :key="memo.id" :style="{ backgroundColor: memo.backgrounColor }">
          <p class="card-content">{{ memo.content }}</p>
          <div class="card-footer">
            <p class="card-date">{{ memo.date }}</p>
            <button class="card-delete-btn" @click="deleteMemo(memo.id)">&times;</button>
          </div>
        </div>
      </div>
    </div>
    <div class="form-overlay" v-if="showForm">
      <div class="form-modal">
        <button class="form-close-btn" @click="showForm = false">&times;</button>
        <p class="form-error" v-if="errorMsg">{{ errorMsg }}</p>
        <textarea name="memo" id="memo" cols="30" rows="10" v-model="newMemo"></textarea>
        <button class="form-save-btn" @click="addMemo">Save</button>
      </div>
    </div>
  </main>
</template>

<style scope>
main {
  height: 100vh;
  width: 100vw;
}

.container {
  max-width: 900px;
  padding: 10px;
  margin: 0 auto;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.header-title {
  font-size: 48px;
  font-size: bold;
  margin-bottom: 25px;
  color: #495a7d;
}

.header-button {
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  border-radius: 100%;
  background-color: #495a7d;
  color: #fff;
}

.card-container {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
}

.card {
  width: 225px;
  height: 225px;
  padding: 10px;
  background-color: #ffa6c1;
  margin-bottom: 20px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.card-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.form-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.77);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
}

.form-modal {
  width: 420px;
  background-color: #fff;
  border-radius: 10px;
  padding: 30px;
  position: relative;
  display: flex;
  flex-direction: column;
}

.form-save-btn {
  padding: 10px 20px;
  font-size: 20px;
  width: 100%;
  background-color: #495a7d;
  border: none;
  cursor: pointer;
  border-radius: 5px;
  margin-top: 15px;
  color: #fff;
}

.form-close-btn {
  position: absolute;
  top: 5px;
  right: 10px;
  width: 30px;
  height: 30px;
  background-color: transparent;
  border: none;
  font-size: 30px;
  cursor: pointer;
}

.card-delete-btn {
  background-color: #ff4200;
  color: #fff;
  width: 30px;
  height: 30px;
  border-radius: 100%;
  border: none;
  font-size: 20px;
  cursor: pointer;
}

.form-error {
  color: red;
  margin-bottom: 10px;
}
</style>