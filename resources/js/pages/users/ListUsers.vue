<script setup>

import axios from 'axios';

import { ref, onMounted, reactive } from 'vue';


const users = ref([]);

const form = reactive({
    name: '',
    email: '',
    password: '',
});

const getUsers = () => {
    axios.get('/api/users')
    .then((response) => {
        users.value = response.data;
    })
}

const createUser = () => {
    axios.post('/api/users', form)
    .then((response) => {
        users.value.unshift(response.data);
        form.name = '';
        form.email = '';
        form.password = '';
        $('#addusers').modal('hide');
    });
}


onMounted(() => {
    getUsers();
});
</script>

<template>
    <div class="content-header">
        <div class="container-fluid">
            <div class="row mb-2">
                <div class="col-sm-6">
                <h1 class="m-0">Users</h1>
            </div>
            <div class="col-sm-6">
                <ol class="breadcrumb float-sm-right">
                    <li class="breadcrumb-item"><a href="#">Home</a></li>
                    <li class="breadcrumb-item active">Users</li>
                </ol>
            </div>
        </div>
    </div>
</div>
<div class="content">
        <div class="container-fluid">
            <div class="card">
                <div class="card-body">
                    <table class="table table table-bordered">
                        <thead>
                            <tr>
                                <th style="width: 10px;">#</th>
                                <th>Name</th>
                                <th>Email</th>
                                <th>Registered Date</th>
                                <th>Role</th>
                                <th>Options</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="(user, index) in users" :key="user.id">
                                <td>{{ index + 1}}</td>
                                <td>{{ user.name }}</td>
                                <td>{{ user.email }}</td>
                                <td></td>
                                <td></td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
            <button class="btn btn-sm btn-primary" data-toggle="modal" data-target="#addusers"> Add New User</button>
        </div>
    </div>



    <div class="modal fade" id="addusers">
        <div class="modal-dialog">
          <div class="modal-content">
            <div class="modal-header">
              <h4 class="modal-title">Add new User</h4>
              <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                <span aria-hidden="true">&times;</span>
              </button>
            </div>
            <div class="modal-body">
                <div class="form-group">
                    <label for="name">Name:</label>
                    <input v-model="form.name" type="text" class="form-control" name="" id="name" placeholder="Enter...">
                </div>
                <div class="form-group">
                    <label for="email">Email:</label>
                    <input v-model="form.email" type="text" class="form-control" name="" id="email" placeholder="Enter...">
                </div>
                <div class="form-group">
                    <label for="password">Password:</label>
                    <input v-model="form.password" type="text" class="form-control" name="" id="password" placeholder="Enter...">
                </div>

            </div>
            <div class="modal-footer justify-content-between">
              <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
              <button type="button" @click="createUser" class="btn btn-primary">Save</button>
            </div>
          </div>
          <!-- /.modal-content -->
        </div>
        <!-- /.modal-dialog -->
      </div>
      <!-- /.modal -->

</template>