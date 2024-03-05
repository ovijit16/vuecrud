<template>
    <div class="container">
        <div class="card">
            <div class="card-header">
                <h4>
                    Students
                    <RouterLink to="/students/create" class="btn btn-primary float-end">
                        Add Student
                    </RouterLink>
                </h4>
            </div>
            <div class="card-body">
                <table class="table table-bordered">
                    <thead>
                        <tr>
                            <th>ID</th>
                            <th>Name</th>
                            <th>Phone</th>
                            <th>Email</th>
                            <th>Address</th>
                            <th>Created At</th>
                            <th>Action</th>
                        </tr>
                    </thead>
                    <tbody v-if="this.students.length > 0">
                        <tr v-for="(student, index) in this.students" :key="index">
                            <td>{{ student.id }}</td>
                            <td>{{ student.name }}</td>
                            <td>{{ student.phone }}</td>
                            <td>{{ student.email }}</td>
                            <td>{{ student.address }}</td>
                            <td>{{ student.created_at }}</td>
                            <td>
                                <RouterLink :to="{ path: '/students/update/' + student.id }" class="btn btn-success ">
                                    Edit
                                </RouterLink>
                                <button type="button" @click="deleteStudent(student.id)"
                                    class="btn btn-danger ">Delete</button>
                            </td>

                        </tr>
                    </tbody>
                    <tbody v-else>
                        <tr>
                            <td colspan="7">Loading...</td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
export default {
    name: 'students',
    data() {
        return {
            students: []
        }
    },
    mounted() {
        // console.log('i am here');
        this.getStudents();
    },
    methods: {
        getStudents() {
            axios.get('http://localhost:8000/api/student').then(res => {
                this.students = res.data
                // console.log(this.students);
            });
        },
        deleteStudent(studentId) {
            // console.log(studentId);
            if (confirm('Are you sure, you want to delete this data')) {
                // console.log(studentId);

                axios.delete(`http://localhost:8000/api/student/${studentId}`)
                    .then(res => {
                        alert(res.data.message);
                        this.getStudents();
                    })
                    .catch(function (error) {
                        if (error.response) {
                            if (error.response.status == 404) {
                                alert(error.response.data.message);
                            }

                        }
                    })
            }
        }
    },
}
</script>

<style></style>
