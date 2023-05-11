<template>
  <br><br>
  <div class="container">
    <span class="me-2">+ Tambah Karyawan</span>
    <button class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#exampleModal">rekrut</button>
    <br>
<h1>Data Karyawan  </h1>
<br>
<table class="table">
  <thead>
    <tr>
      <th scope="col">No</th>
      <th scope="col">Foto</th>
      <th scope="col">Nama depan</th>
      <th scope="col">Nama Belakang</th>
      <th scope="col">Email</th>
      <th scope="col">Interaksi</th>
    </tr>
  </thead>
  <tbody v-for="(orang, i) in karyawan" :key="i">
    <tr>
      <th scope="row">{{ i + 1 }}</th>
      <td><img :src="orang.avatar" alt="orang" data-bs-toggle="modal" data-bs-target="#detail" @click="e => viewKaryawan(orang.id)" style="width: 50px;"></td>
      <td>{{orang.first_name}}</td>
      <td>{{orang.last_name}}</td>
      <td>{{orang.email}}</td>
      <td>
        <button class="btn btn-warning me-2">ubah</button>
        <button class="btn btn-outline-danger editbuttonpecat" @click="() =>deleteKaryawan(orang.id)">pecat</button>
      </td>
    </tr>
  </tbody>
</table>
</div>
<!-- Modal -->
<div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="exampleModalLabel">Form Rekrut Karyawan</h5>
        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
      </div>
      <div class="modal-body">
        <form @submit.prevent="submitForm">
  <div class="mb-3">
    <label for="foto" class="form-label">Foto</label>
    <input type="file" class="form-control" @change="(e) => this.target = e.target.files[0]" id="foto">
  </div>
  <div class="mb-3">
    <label for="nama_belakang" class="form-label">Nama</label>
    <input type="text" class="form-control" v-model="name" id="name"  required>
  </div>
  <div class="mb-3">
    <label for="exampleInputEmail1" class="form-label">Email address</label>
    <input type="email" class="form-control" id="email" aria-describedby="emailHelp">
  </div>
  <button type="submit" class="btn btn-primary">Submit</button>
</form>
      </div>
    </div>
  </div>
</div>
<!-- Modal -->
<div class="modal fade" id="detail" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="exampleModalLabel">Detail Data</h5>
        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
      </div>
      <div class="modal-body">
        <ul class="list-group">
        <li class="list-group-item"><img :src="davatar" alt="orang"></li>
        <li class="list-group-item">{{ dname }}</li>
        <li class="list-group-item">{{ demail}}</li>
        </ul>
      </div>
    </div>
  </div>
</div>
</template>

<script>
// fetch("https://reqres.in/api/users?page=2")
// .then(res => res.json()) //chaining
// .then(res => console.log(res))

import axios from 'axios'

export default {
  name: 'App',
  data(){
    return{
      karyawan : [],
      name: '',
      email: '',
      avatar: null,
      dname: '',
      demail: '',
      davatar:'',
    }
  },
  mounted(){
    //get metode ambil data
    axios.get("https://reqres.in/api/users?page=2")
    .then(res => (this.karyawan = res.data.data, console.log(res.data.data)))
  },
  methods:{
    submitForm(){
      let formData =new FormData();

      formData.append('name', this.name)
      formData.append('email', this.email)
      formData.append('avatar', this.avatar)

      axios.post("https://reqres.in/api/users",formData, {
        headers:{
          'Content-Type' : 'multipart/form-data'
        }
      })
      .then(res => console.log("Yeay Sukses :", res))
      .catch(err => console.error("error cuy :", err))
    },
    deleteKaryawan(hapusdata){
      axios.delete('https://reqres.in/api/users/' + hapusdata)
      .then(res => (console.log("Terpecat"),console.log(res)))
    },
    viewKaryawan(hapusdata){
      axios.get('https://reqres.in/api/users/' + hapusdata)
      .then(res => {
        this.dname= res.data.data.first_name +''+ res.data.data.last_name
        this.demail = res.data.data.email
        this.davatar = res.data.data.avatar
      })
    }
  }
}
</script>
<style>
.editbuttonpecat{
  margin-top: 0px;
}
</style>