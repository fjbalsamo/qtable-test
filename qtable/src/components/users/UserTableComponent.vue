<template>
  <div>
    <q-table title="Users" :data="usersList" :columns="columns" row-key="id">
      <!-- header -->
      <template v-slot:header="props">
        <q-tr :props="props">
          <q-th auto-width />
          <q-th v-for="col in props.cols" :key="col.name" :props="props">
            {{ col.label }}
          </q-th>
        </q-tr>
      </template>
      <!-- body -->
      <template v-slot:body="props">
        <q-tr :props="props">
          <q-td auto-width>
            <q-btn
              size="sm"
              color="accent"
              round
              dense
              @click="toggle(props)"
              :icon="props.expand ? 'remove' : 'add'"
            />
          </q-td>
          <q-td v-for="col in props.cols" :key="col.name" :props="props">
            {{ col.value }}
          </q-td>
        </q-tr>
        <q-tr v-show="props.expand" :props="props">
          <expand-post-component :ref="`posts-${props.key}`" :posts="props.row.posts" />
        </q-tr>
      </template>
    </q-table>
  </div>
</template>

<script>
import axios from 'axios'
import ExpandPostComponent from '../posts/ExpandPostComponent'

export default {
  name: "UserTableComponent",
  components: {
      ExpandPostComponent
  },
  data() {
    return {
      usersList: [],
      columns: [       
        {
          name: "name",
          required: true,
          label: "Name",
          align: "left",
          field: row => row.name,
          format: val => `${val}`,
          sortable: true
        },
        {
          name: "username",
          required: true,
          label: "Username",
          align: "left",
          field: row => row.username,
          format: val => `${val}`,
          sortable: true
        },
        {
          name: "email",
          required: true,
          label: "email",
          align: "left",
          field: row => row.email,
          format: val => `${val}`,
          sortable: true
        }
      ]
    };
  },
  methods: {
      async loadUsers () {
          try {
              let response = await axios.get('https://jsonplaceholder.typicode.com/users')
              this.usersList = response.data.map(el => ({ ...el, posts: [] }))
          } catch (error) {
              console.error('loadUsers:\n', error)
          }
      },
      async loadUserPost(userSelected){
          try {
              let response = await axios.get('https://jsonplaceholder.typicode.com/posts')
              const posts = response.data;              
              return posts.filter(el => el.userId === userSelected.id)
          } catch (error) {
              console.error('loadUserPost', error)
              return []
          }

      },
      async toggle(tableProps){
          try {              
              tableProps.expand = ! tableProps.expand
              const row = tableProps.row
              const userFilter = this.usersList.filter(el => el.id === row.id)
              if (userFilter.length === 1) {
                  const userSelected = userFilter[0]
                  const userIndex = this.usersList.indexOf(userSelected)
                  if (userSelected.posts.length === 0) {
                      const response = await axios.get(`https://jsonplaceholder.typicode.com/posts`)
                      const allPosts = response.data
                      this.usersList[userIndex].posts=allPosts.filter(el => el.userId === userSelected.id)
                  }
              }              
          } catch (error) {
              console.error('toggle', error)
          }
      }
  },
  created () {
      this.loadUsers()
  }
};
</script>
