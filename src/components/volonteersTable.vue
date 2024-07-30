<template>
  <table>
    <thead>
      <tr>
        <th v-for="col in columns" :key="col.key">
          <slot name="tableHeader" :col="col"></slot>
        </th>
      </tr>
    </thead>

    <tbody>
      <tr v-for="volonteer in filteredVolonteers" :key="volonteer.name">
        <td v-for="col in columns" :key="col.key">
          <slot v-if="col.key === 'permission' && volonteer.isAdult" name="permissionNotRequired"></slot>
          <slot v-else-if="col.key === 'permission'" name="permissionRequired" :volonteer="volonteer"></slot>
          <slot v-else name="tableData" :col="col" :volonteer="volonteer"></slot>
        </td>
      </tr>
    </tbody>
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
    permissions: {
      type: Object,
      default: {},
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
        id: volonteer.id,
        hasPermission: props.permissions[volonteer.id],
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
      {
        label: "Разрешение",
        key: "permission",
      },
    ]
  
    return {
      columns,
      filteredVolonteers,
    }
  }
})
</script>
