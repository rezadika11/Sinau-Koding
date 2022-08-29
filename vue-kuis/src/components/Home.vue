<template>
<div class="container mt-3">
<p class="fs-3">Daftar Pengguna</p>
<div class="d-flex justify-content-end">
<button type="button" class="btn btn-success mb-3" data-bs-toggle="modal" data-bs-target="#addModal">Tambah Pengguna</button>
</div>
<!-- Add Modal -->
<div class="modal fade" id="addModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="exampleModalLabel">Tambah Pengguna</h5>
        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
      </div>
      <div class="modal-body">
        <div class="mb-3">
            <label for="name" class="form-label">Name</label>
            <input type="text" class="form-control" id="name" v-model="addPengguna.name">
        </div>
         <div class="mb-3">
            <label for="email" class="form-label">Email</label>
            <input type="text" class="form-control" id="email" v-model="addPengguna.email">
        </div>
         <div class="mb-3">
            <label for="gender" class="form-label">Gender</label>
            <input type="text" class="form-control" id="gender" v-model="addPengguna.gender">
        </div>
         <div class="mb-3">
            <label for="status" class="form-label">Status</label>
            <input type="text" class="form-control" id="status" v-model="addPengguna.status">
        </div>
      </div>
      <div class="modal-footer">
        <button v-on:click="tambahPengguna" class="btn btn-primary mb-3">Simpan</button><br>
      </div>
    </div>
  </div>
</div>

 <table class="table table-striped table-bordered" >
    <thead>
        <tr>
            <th>ID</th>
            <th>Name</th>
            <th>Email</th>
            <th>Gender</th>
            <th>Status</th>
            <th>Aksi</th>
        </tr>
    </thead>
    <tbody>
        <tr v-for="data in pengguna" :key="data.id">
            <td>{{ data.id }}</td>
            <td>{{ data.name }}</td>
            <td>{{ data.email }}</td>
            <td>{{ data.gender }}</td>
            <td>{{ data.status }}</td>
            <td>
                <button v-on:click="viewPengguna(data.id)" class="btn btn-info btn-sm" data-bs-toggle="modal" data-bs-target="#viewModal">
                 View</button> |
                <a :to="'/update/'+data.id" type="button" class="btn btn-warning btn-sm" data-bs-toggle="modal" data-bs-target="#updateModal">
                 Update</a> |
                <button v-on:click="deletePengguna(data.id)"  class="btn btn-sm btn-danger">Delete</button>
            </td>
        </tr>
    </tbody>
    </table>
    <!-- View Modal -->
<div class="modal fade" id="viewModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="exampleModalLabel">Detail Pengguna</h5>
        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
      </div>
      <div class="modal-body">
        <table class="table table-striped">
            <tbody>
            <tr>
                <td>ID</td>
                <td>: {{  detailPengguna['id']  }}</td>
            </tr>
             <tr>
                <td>Name</td>
                <td>: {{  detailPengguna['name']  }}</td>
            </tr>
              <tr>
                <td>Email</td>
                <td>: {{  detailPengguna['email']  }}</td>
            </tr>
              <tr>
                <td>Gender</td>
                <td>: {{  detailPengguna['gender']  }}</td>
            </tr>
              <tr>
                <td>Status</td>
                <td>: {{  detailPengguna['status']  }}</td>
            </tr>
            </tbody>
        </table>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
      </div>
    </div>
  </div>
</div>
    <!-- Update Modal -->
<div class="modal fade" id="updateModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="exampleModalLabel">Update Pengguna</h5>
        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
      </div>
      <div class="modal-body">
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
        <button type="button" class="btn btn-primary">Save changes</button>
      </div>
    </div>
  </div>
</div>
</div>
   
</template>
<script>
import axios from 'axios' 
const url = 'https://gorest.co.in/public/v2/users/'
const token = '7aa336f2b419e808fcca8ecaf5535696e098a2cf0d22befb3785bb0e30d345d7'      
export default ({
    name:"pengguna",
    data() {
        return {
            pengguna: [],
              addPengguna :{
                name:'',
                email:'',
                gender:'',
                status:'',
            },
            
            detailPengguna: [],

        }
        
    },
    methods:{
    async viewPengguna(id){
         let resultView = await axios.get(url+id,{
                        headers: {
                        Authorization: `Bearer ${token}`,
                         },
             })
                this.detailPengguna=resultView.data
    },
       async deletePengguna(id){
            let resultDelete = await axios.delete(url+id,{
                  headers: {
                        Authorization: `Bearer ${token}`,
                         },
            });
            if(resultDelete.status == 204){
                this.loadData()
            }
        },
        async loadData(){
             let res = await axios.get(url,{
                        headers: {
                        Authorization: `Bearer ${token}`,
                         },
             })
                    this.pengguna=res.data
        },

     async tambahPengguna()
     {
            let resultTambah =  await axios.post(url,{
                name: this.addPengguna.name,
                email: this.addPengguna.email,
                gender: this.addPengguna.gender,
                status: this.addPengguna.status,
            },{
             headers: {
                'Content-Type': 'application/json',
                 Authorization:`Bearer ${token}`,
             }
                
            });

            if(resultTambah.status == 201){
                 this.$router.push({name:'home'})
            }
        }
    },

   async mounted()
    {
     this.loadData();
    }
        
    
})
</script>
