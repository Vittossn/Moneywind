<template>
  <button @click="showModal = true" class="button2">Nuevo Movimiento</button>
  <Teleport to="#app">
    <MyModal v-show="showModal" @close="showModal = false">
      <form @submit.prevent="submit">
        <div class="field">
          <label>Titulo</label>
          <input type="text" v-model="title" />
        </div>
        <div class="field">
          <label>Monto</label>
          <input type="number" v-model="amount" />
        </div>
        <div class="field">
          <label> Descripcion </label>
          <textarea row="8" v-model="description"></textarea>
        </div>
        <div class="field">
          <label class="radio-label">
            <div class="input_radio">
              <input type="radio" v-model="movementType" value="Ingreso" />
              <span>Ingreso</span>
            </div>
            <div class="input_radio">
              <input type="radio" v-model="movementType" value="Gasto" />
              <span>Gasto</span>
            </div>
          </label>
        </div>
        <div class="action">
          <button>Realizar Movimiento</button>
        </div>
      </form>
    </MyModal>
  </Teleport>
</template>
<script setup>
import MyModal from "./MyModal.vue";
import { ref, defineEmits } from "vue";
const showModal = ref(false);
const title = ref("");
const amount = ref(0);
const description = ref("");
const movementType = ref("Ingreso");
const emit = defineEmits(["create"]);
const submit = () => {
  showModal.value = !showModal.value;
  emit("create", {
    title: title.value,
    description: description.value,
    amount: movementType.value === "Ingreso" ? amount.value : -amount.value,
    time: new Date(),
    id: new Date(),
  });
  (title.value = ""),
    (description.value = ""),
    (amount.value = 0),
    (movementType.value = "Ingreso");
};
</script>
<style scoped>
.button2 {
  margin-top: 12%;
}
button {
  color: white;
  font-size: 1.25rem;
  background-color: #242424;
  border: none;
  width: 100%;
  padding: 24px 60px;
  border-radius: 60px;
  box-sizing: border-box;
}
form {
  font-size: 1.24rem;
  width: 100%;
}
form .action {
  padding: 0 24px;
}
.field {
  display: flex;
  justify-content: space-between;
  flex-direction: column;
  padding: 16px 24px;
}
label {
  margin-bottom: 8px;
}
input,
textarea {
  font-size: 1.24rem;
  border: 2px solid #525252;
  border-radius: 8px;
  padding: 8px;
  text-align: left;
}
.radio-label {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 8px;
}
.radio-label span {
  margin-top: 4px;
}
.input_radio {
  width: 20%;
}
input[type="radio"] {
  appearance: none;
  width: 1.24rem;
  height: 1.24rem;
  color: blue;
  border: 2px solid #33f;
  border-radius: 50%;
  margin-left: 17%;
}
input[type="radio"]:checked {
  background-color: #649;
}
.radio-label {
  height: 50%;
}
</style>
