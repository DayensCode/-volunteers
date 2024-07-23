<template>
  <table>
    <tr>
      <th>ФИО</th>
      <th>Возраст</th>
      <th>Подразделение</th>
      <th>Ответственный руководитель</th>
      <th>Мед. противопоказания</th>
    </tr>
    <tr v-for="volonteer in filteredVolonteers" :key="volonteer.name">
      <td>{{ volonteer.name }}</td>
      <td>{{ volonteer.age }}</td>
      <td>{{ volonteer.directions }}</td>
      <td>{{ volonteer.leader }}</td>
      <td>{{ volonteer.medicalContraindications }}</td>
    </tr>
  </table>
</template>

<script>
import { defineComponent, computed } from "vue"
import { Volonteers } from "@/const/volunteers"

export default defineComponent({
  name: "VolonteersTable",
  props: {
    searchQuery: {
      type: String,
      default: "",
    },
    showUnderage: {
      type: Boolean,
      default: false,
    },
  },
  setup(props) {
    const formattedVolonteers = computed(() => {
    const values = Volonteers.map((volonteer) => ({
        name: volonteer.first_name + ' ' + volonteer.last_name,
        age: volonteer.age,
        directions: volonteer.directions,
        leader: volonteer.leader,
        medicalContraindications: volonteer.medicalContraindications.join(', '),
        isAdult: volonteer.age >= 18,
    }))

    return values
    })

    const filteredVolonteers = computed(() => {
      return formattedVolonteers.value.filter(volonteer => {

        if (props.searchQuery && props.showUnderage) {
          return volonteer.name.toLowerCase().includes(props.searchQuery.toLowerCase())
        } else if (props.searchQuery) {
          return volonteer.isAdult ? volonteer.name.toLowerCase().includes(props.searchQuery.toLowerCase()) : false
        } else if (props.showUnderage) {
          return volonteer
        } else {
          return volonteer.isAdult
        }
      })
    })
  
    return {
      filteredVolonteers,
    }
  }
})
</script>
