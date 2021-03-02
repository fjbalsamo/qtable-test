<template>
  <q-page>
      <user-table-component class="q-pa-xl" ref="userTable" />
  </q-page>
</template>

<script>
import UserTableComponent from '../components/users/UserTableComponent'

export default {
  name: 'PageIndex',
  components: {
    UserTableComponent
  }
}
</script>
