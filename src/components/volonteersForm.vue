<template>
  <form>
    <input type="text" placeholder="Имя волонтера" v-model="name">
    <input type="checkbox" id="isUnderage" v-model="isUnderage">
    <label for="isUnderage">показать несовершеннолетних</label>
  </form>
</template>

<script>
import { defineComponent, ref, watch } from "vue"

export default defineComponent({
  name: "VolonteersForm",
  emits: ["searchName", "toggleUnderage"],
  setup(_, { emit }) {
    const name = ref("")
    const isUnderage = ref(false)

    watch(() => name.value, () => {
      emit("searchName", name.value)
    })

    watch(() => isUnderage.value, () => {
      emit("toggleUnderage", isUnderage.value)
    })

    return {
      name,
      isUnderage,
    }
  }
})
</script>

<style lang="scss" scoped>
form {
  margin-bottom: 40px;

  input[type=text] {
    width: 800px;
    padding: 10px 30px;
    margin-right: 50px;
  }

  input[type=checkbox] {
    margin-right: 10px;
    cursor: pointer;
  }

  label {
    cursor: pointer;
  }
}
</style>
