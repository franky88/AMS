<template>
  <div id="app">
    <div class="modal fade bd-example-modal-lg" tabindex="-1" role="dialog" aria-labelledby="myLargeModalLabel" aria-hidden="true">
      <div class="modal-dialog modal-lg">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel">Update Student Details</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <div class="row">
              <div class="col-sm-6">
                <h4>{{student.get_fullname}}</h4>
              </div>
              <div class="col-sm-6">
                <div class="card bg-light">
                  <div class="card-body">
                    <h3 class="card-title">Edit Student Form</h3>
                    <hr>
                    <form @submit.prevent="editStudent">
                      <div class="form-group">
                        <label for="firstName">First Name</label>
                        <input type="text" class="form-control" placeholder="First Name" v-model="student.first_name" required='required'>
                      </div>
                      <div class="form-group">
                        <label for="lastName">Last Name</label>
                        <input type="text" class="form-control" placeholder="Last Name" v-model="student.last_name" required='required'>
                      </div>
                      <div class="form-group">
                        <label for="middleName">Middle Name</label>
                        <input type="text" class="form-control" placeholder="Middle Name" v-model="student.middle_name">
                      </div>
                      <div class="form-group">
                        <label for="extensionName">Extension Name</label>
                        <input type="text" class="form-control" placeholder="Extension Name" v-model="student.extension_name">
                      </div>
                      <div class="form-group">
                        <label for="birthDate">Birth Date</label>
                        <input type="date" class="form-control" placeholder="Birth Date" v-model="student.birth_date" required='required'>
                      </div>
                      <div class="form-group">
                        <label for="parentContact">Parent Mobile Contact</label>
                        <input type="text" class="form-control" placeholder="Parent Contact" v-model="student.parent_contact" required='required'>
                      </div>
                      <button class="btn btn-outline-primary">Update</button> | 
                      <button type="button" class="btn btn-danger" data-dismiss="modal">Cancel</button>
                    </form>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="row">
      <div class="col-sm-4">
        <div class="card bg-light">
          <div class="card-header">
            Student Initial Details
          </div>
          <div class="card-body">
            <h3 class="card-title">Create Student Form</h3>
            <hr>
            <form @submit.prevent="submitForm">
              <div class="form-group">
                <label for="firstName">First Name</label>
                <input type="text" class="form-control" placeholder="First Name" v-model="student.first_name" required='required'>
              </div>
              <div class="form-group">
                <label for="lastName">Last Name</label>
                <input type="text" class="form-control" placeholder="Last Name" v-model="student.last_name" required='required'>
              </div>
              <div class="form-group">
                <label for="middleName">Middle Name</label>
                <input type="text" class="form-control" placeholder="Middle Name" v-model="student.middle_name">
              </div>
              <div class="form-group">
                <label for="extensionName">Extension Name</label>
                <input type="text" class="form-control" placeholder="Extension Name" v-model="student.extension_name">
              </div>
              <div class="form-group">
                <label for="birthDate">Birth Date</label>
                <input type="date" class="form-control" placeholder="Birth Date" v-model="student.birth_date" required='required'>
              </div>
              <div class="form-group">
                <label for="parentContact">Parent Mobile Contact</label>
                <input type="text" class="form-control" placeholder="Parent Contact" v-model="student.parent_contact" required='required'>
              </div>
              <div class="form-group">
                <label for="parentEmail">Parent Email Address</label>
                <input type="email" class="form-control" placeholder="Parent Email" v-model="student.parent_email">
              </div>
              <button class="btn btn-outline-primary">Submit</button>
            </form>
          </div>
        </div>
      </div>
      <div class="col-sm-8">
        <div class="card">
          <div class="card-body">
            <h3 class="card-title">Student list</h3>
            <input class="form-control mr-sm-2" v-model="search" type="search" placeholder="Search Student" aria-label="Search">
            <br>
            <table class="table table-sm table-hover">
              <thead>
                <tr>
                  <th>Full Name</th>
                  <th>Parent Contact</th>
                  <th>Actions</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="student in filteredStudents" :key="student.id" @dblclick="$data.student = student">
                  <!-- <td>{{student.first_name}}</td> -->
                  <td>
                    <strong>{{student.last_name}}, {{student.first_name}}</strong>
                    <br>
                    <small class="text-muted">Date registered: {{student.date_register|formatDate}}</small>
                  </td>
                  <td>{{student.parent_contact}}</td>
                  <td> 
                    <button class="btn btn-outline-danger btn-sm mx-1" @click="deleteStudent(student)"><i class="fas fa-trash-alt"></i></button>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
        
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'Students',
  data () {
    return {
      student: {},
      students: [],
      search: '',
    }
  },

  async created(){
    await this.getStudents();
  },
  methods: {
    submitForm(){
      if (this.student.id === undefined){
        this.createStudent();
      } else {
        this.editStudent();
      }
    },
    async getStudents(){
      var response = await fetch('http://localhost:8000/api/students/');
      this.students = await response.json();
    },
    async createStudent(){
      await this.getStudents();
      await fetch('http://localhost:8000/api/students/', {
        method: 'post',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(this.student)
      });
      await this.getStudents();
      this.student = {};
    },
    async editStudent(){
      await this.getStudents();
      await fetch(`http://localhost:8000/api/students/${this.student.id}/`, {
        method: 'put',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(this.student)
      });
      await this.getStudents();
      this.student = {};
    },
    async deleteStudent(student){
      await this.getStudents();
      await fetch(`http://localhost:8000/api/students/${student.id}/`, {
        method: 'delete',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(this.student)
      });
      await this.getStudents();
    }
  },
  computed: {
    filteredStudents: function(){
      return this.students.filter((student) => {
        return student.get_fullname.match(this.search);
      });
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  margin-top: 60px;
}
body {
  background-color: #D8E8E6;
}
</style>
