<template>
  <div class="app">
  <div class="snia row">
  <h2 style="text-align:center;">Selamat Datang Admin...</h2>
  <div class="one">
  
      <form @submit.prevent="add">
    <h2>Form Peminjaman Buku</h2>
      <label for="">Nama Siswa</label><br>
      <input type="text" v-model="form.nama" > <br><br>
      <label for="">Judul Buku</label><br>
      <input type="text" v-model="form.judul" ><br><br>
      <label for="">Tanggal Pinjam</label><br>
      <input type="date" v-model="form.pinjam"><br><br>
      <label for="">Tanggal Pengambilan</label><br>
      <input type="date" v-model="form.kembali"><br><br>

        <button  type="submit" v-show="!updateSubmit">
         Add Peminjaman</button> <!--jika tidak update maka tombol update tidak muncul --> 
          <button type="button" v-show="updateSubmit" @click="update(form)">Update</button>
          <!--jika tombol edit di klik maka tombol add akan berubah menjadi update -->
    </form><br><br>
    </div>

    <div class="two">
    <h2>Tabel Daftar Buku</h2>

     <div class="satu row">
    <b><label for="quantity">show</label>
    <input type="number" id="quantity" name="quantity" min="0" max="50"></b></div>
    <div class="dua">
    <b><label for="quantity">search</label>
    <input type="texty" id="quantity" name="quantity" min="0" max="50"></b></div>
    <br> <br>

    
<table>
  <thead>
  <tr>
  <th style="width:110px;">No</th>
  <th style="width:80px;">Nama Siswa</th>
  <th style="width:80px;">Judul Buku</th>
  <th style="width:105px;">Tanggal Pinjam</th>
  <th style="width:145px;">Tanggal Pengambilan</th>
  <th >Aksi</th>
  </tr>
  </thead>
  <br>
  <tbody v-for="user in users" :key="user.id">
  <tr>
  <td>{{ user.id+1 }}</td>
  <td>{{ user.nama }}</td>
  <td>{{ user.judul }}</td>
  <td>{{ user.pinjam }}</td>
  <td>{{ user.kembali }}</td>
  <td><button class="btn" @click="edit(user)">Perpanjang</button><br><button class="buton" @click="del(user)">Kembali</button>
</td>
  </tr>
  </tbody>
  </table>
  <br><br>
  <ul>
  <li>Jika meminjam harus dikembalikan pada waktunya</li>
  <li>Keterlambatan pengembalian buku dikenakan denda Rp. 2000,-/buku/hari.</li>
  </ul>
  </div>
  </div>
  </div>
</template>
<script>
/* eslint-disable */ 
import axios from 'axios'
export default {
  data(){
    return{
        users: [],
        form: {
          nama: '',
          judul: '',
          pinjam:'',
          kembali:'',
        },
        users: '',
        updateSubmit: false
    }
  },
  mounted() {
    this.load()
  },
  methods: {
    load(){
        axios.get('http://localhost:3000/users').then(res => {
        this.users = res.data //respon dari rest api dimasukan ke users
      }).catch ((err) => {
        console.log(err);
      })
    },
     add(){
      axios.post('http://localhost:3000/users/', this.form).then(res => {
          this.load()
          this.form.nama = ''
          this.form.judul = ''
          this.form.pinjam = ''
          this.form.kembali = ''
      })
 },
  edit(user){ 
        this.updateSubmit = true
        this.form.id = user.id 
        this.form.nama = user.nama 
        this.form.judul = user.judul
        this.form.pinjam = user.pinjam
        this.form.kembali = user.kembali
 },
update(form){
        return axios.put('http://localhost:3000/users/' + form.id , {nama: this.form.nama, judul: this.form.judul, pinjam: this.form.pinjam, kembali: this.form.kembali}).then(res => {
        this.load()
        this.form.id = ''
        this.form.nama = ''
        this.form.judul = ''
        this.form.pinjam = ''
        this.form.kembali = ''
        this.updateSubmit = false
      }).catch((err) => {
        console.log(err);
        
      })
    },
      del(user){
      axios.delete('http://localhost:3000/users/' + user.id).then(res =>{
          this.load()
          let index = this.users.indexOf(form.judul)
          this.users.splice(index,1)
      })
  }


   }
  }
</script>
<style scoped>
.app {
background-color: lightblue;
}
table{
margin-left:auto;
text-align: center;
border-collapse:collapse;
width:800px
}


td {
border: 1px solid black;
border-left: none;
border-right: none;
background-color: whitesmoke;
}

.one {
float: left;
padding: 15px;
}

.two {
float: left;
padding: 15px;
width: 800px;
margin-bottom: 50px;
}

.row:after {
content: "";
display: table;
clear: both;
}

.satu {
float: left;
width: 20%;
padding: 10px;
}

.dua {
float: right;
width: 40%;
padding: 10px;
}

.btn {
background-color: #4CAF50;
  border: none;
  color: white;
  padding: 6px 10px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 14px;
  margin: 4px 2px;
  cursor: pointer;
}

.buton {
background-color: #c71f1f;
  border: none;
  color: white;
  padding: 6px 10px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 14px;
  margin: 4px 2px;
  cursor: pointer;

}

input[type=text], select {
  width: 350px;
  padding: 8px 15px;
  margin: 8px 0;
  display: inline-block;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
  }

  input[type=date], select {
  width: 350px;
  padding: 8px 15px;
  margin: 8px 0;
  display: inline-block;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
  }

button[type=submit] {
  width: 100%;
  background-color: rgb(80, 172, 202);
  color: white;
  padding: 14px 20px;
  margin: 8px 0;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button[type=button] {
  width: 100%;
  background-color: rgb(80, 172, 202);
  color: white;
  padding: 14px 20px;
  margin: 8px 0;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}


</style>