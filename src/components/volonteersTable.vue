<template>
  <table>
    <tr>
      <th v-for="col in columns" :key="col.key">
          {{ col.label }}
      </th>
    </tr>
    <tr v-for="volonteer in filteredVolonteers" :key="volonteer.name">
      <td v-for="col in columns" :key="col.key">{{ volonteer[col.key] }}</td>
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
          return !volonteer.isAdult ? volonteer.name.toLowerCase().includes(props.searchQuery.toLowerCase()) : false
        } else if (props.searchQuery) {
          return volonteer.isAdult ? volonteer.name.toLowerCase().includes(props.searchQuery.toLowerCase()) : false
        } else if (props.showUnderage) {
          return !volonteer.isAdult
        } else {
          return volonteer.isAdult
        }
      })
    })

    const columns = [
      {
        label: "ФИО",
        key: "name",
      },
      {
        label: "Возраст",
        key: "age",
      },
      {
        label: "Подразделение",
        key: "directions",
      },
      {
        label: "Ответственный руководитель",
        key: "leader",
      },
      {
        label: "Мед. противопоказания",
        key: "medicalContraindications",
      },
    ]
  
    return {
      columns,
      filteredVolonteers,
    }
  }
})
</script>
