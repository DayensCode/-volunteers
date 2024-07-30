<template>
  <div class="modal" v-if="showModal">
    <div class="modal-content">
      <span @click="closeModal">&times;</span>
      <h2>Разрешение от родителей/опекунов на волонтерство на фестивале получено</h2>
      <div>
        <button>Подтвердить</button>
        <button @click="closeModal">Отмена</button>
      </div>
    </div>
  </div>
</template>
  
<script>
import { defineComponent } from "vue"

export default defineComponent({
  name: "PermissionModal",
  props: {
    showModal: {
      type: Boolean,
      required: true,
    },
  },
  emits: ["close"],
  setup(_, { emit }) {
    const closeModal = () => {
      emit("close")
    }

    return {
      closeModal,
    }
  }
})
</script>
  
<style lang="scss" scoped>
.modal {
  display: flex;
  justify-content: flex-end;
  position: fixed;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
}

.modal-content {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: flex-end;
  background-color: white;
  padding: 20px 40px 120px;
  max-width: 600px;

  span {
    cursor: pointer;
  }

  button {
    margin-right: 12px;
    cursor: pointer;

    &:last-child {
      margin-right: 0; 
    }
  }
}
</style>
  