<template>
  <div id="app">
    <div class="row">
      <div class="col-sm-12">
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
    </div>
  </div>
</template>

<script>
export default {
  name: 'CreateAndEditStudent',
  data () {
    return {
      student: {},
      students: []
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
</style>
