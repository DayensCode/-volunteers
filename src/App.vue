<template>
  <header>
    <div class="container">
      <h1>Учет волонтеров</h1>
    </div>
  </header>

  <main>
    <div class="container">
      <VolonteersForm @toggleUnderage="handleToggleUnderage" @searchName="handleSearchName" />
      <VolonteersTable :showUnderage="showUnderage" :searchQuery="searchQuery">

        <template #tableHeader="{ col }">{{ col.label }}</template>
        <template #tableData="{ col, volonteer }">{{ volonteer[col.key] }}</template>

        <template #permissionNotRequired>не требуется</template>
        <template #permissionRequired>
          <button @click="requestPermission">Подтвердить</button>
          <template>получено</template>
        </template>

      </VolonteersTable>


      <PermissionModal :showModal="showModal" @close="closeModal" />
    </div>
  </main>
</template>

<script>
import { defineComponent, ref } from "vue"
import VolonteersForm from "@/components/volonteersForm.vue"
import VolonteersTable from "@/components/volonteersTable.vue"
import PermissionModal from "@/components/permissionModal.vue"

export default defineComponent({
  components: {
    VolonteersForm,
    VolonteersTable,
    PermissionModal,
  },
  setup() {
    const searchQuery = ref("")
    const showUnderage = ref(false)
    const showModal = ref(false)

    const handleSearchName = (name) => {
      searchQuery.value = name
    }

    const handleToggleUnderage = (isUnderage) => {
      showUnderage.value = isUnderage
    }

    const requestPermission = () => {
      showModal.value = true
    }

    const closeModal = () => {
      showModal.value = false
    }

    return {
      searchQuery,
      showUnderage,
      showModal,
      handleSearchName,
      handleToggleUnderage,
      requestPermission,
      closeModal,
    }
  }
})
</script>

<style lang="scss" scoped>
header {
  padding: 20px 0;
  background-color: darkblue;
  color: white;
}

main {
  height: 100vh;
  padding: 120px 0;

  button {
    cursor: pointer;
  }
}
</style>
