<template>
  <header>
    <div class="container">
      <h1>Учет волонтеров</h1>
    </div>
  </header>

  <main>
    <div class="container">
      <VolonteersForm @toggleUnderage="handleToggleUnderage" @searchName="handleSearchName" />
      <VolonteersTable :showUnderage="showUnderage" :searchQuery="searchQuery" />
    </div>
  </main>
</template>

<script>
import { defineComponent, ref } from "vue"
import VolonteersForm from "@/components/volonteersForm.vue"
import VolonteersTable from "@/components/volonteersTable.vue"

export default defineComponent({
  components: {
    VolonteersForm,
    VolonteersTable,
  },
  setup() {
    const searchQuery = ref("")
    const showUnderage = ref(false)

    const handleSearchName = (name) => {
      searchQuery.value = name
    }

    const handleToggleUnderage = (isUnderage) => {
      showUnderage.value = isUnderage
    }

    return {
      searchQuery,
      showUnderage,
      handleSearchName,
      handleToggleUnderage,
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
}
</style>
