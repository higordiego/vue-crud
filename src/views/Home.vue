<template>
  <div class="home" style="margin-top: 150px; margin-left: 200px">
    <form @submit.prevent="registry({name, email, age})">
      <input type="text" v-model="name" placeholder="Name" />
      <input type="email" v-model="email" placeholder="Email" />
      <input type="age" v-model="age" placeholder="Idade" />
      <button type="submit">Enviar</button>
    </form>
    <br />
    <table>
      <tr>
        <th>_id</th>
        <th>Name</th>
        <th>Email</th>
        <th>Idade</th>
        <th>Ações</th>
      </tr>
      <tr v-for="item in angularjs" :key="item._id">
        <td>{{item._id}}</td>
        <td>{{item.name}}</td>
        <td>{{item.email}}</td>
        <td>{{item.age}}</td>
        <td>
          <button style="margin: 0px 8px;" @click="update(item)">Alterar</button>
          <button style="margin: 0px 8px;" @click="remove(item._id)">Excluir</button>
        </td>
      </tr>
    </table>
  </div>
</template>
<script>
import axios from 'axios'
export default {
  name: 'home',
  data: () => ({
    name: '',
    email: '',
    age: '',
    angularjs: []
  }),
  // data () {
  //   return {
  //     name: '',
  //     email: '',
  //     age: ''
  //   }
  // },
  components: {},
  mounted () {
    this.listAll()
  },
  methods: {
    async registry (value) {
      try {
        await axios.post('https://aef9bcfb.ngrok.io', value)
        alert('cadastrado com sucesso!')
      } catch (err) {
        alert(err.data)
      }
    },
    async remove (id) {
      await axios.delete(`https://aef9bcfb.ngrok.io/${id}`)
    },
    async update (value) {
      try {
        await axios.put('https://aef9bcfb.ngrok.io', value)
        await this.listAll()
      } catch (err) {
        alert(err.message)
      }
    },
    async listAll () {
      try {
        const { data } = await axios.get('https://aef9bcfb.ngrok.io')
        console.log(data)
        this.angularjs = data
      } catch (err) {
        alert(err.message)
      }
    }
  }
}
</script>
