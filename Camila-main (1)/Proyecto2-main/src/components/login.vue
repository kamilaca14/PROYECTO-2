<template>
  <header>
    <div class="header-logo">
      <img src="../assets/logo_ULEAM_2017_horizontal.png" alt="" id="logo">
      <h2>{{ title }}</h2>
    </div>
  </header>  
  <main>
    <div class="form-container">
      <div class="form-header">
        <img src="../assets/logo_ULEAM_2017_horizontal.png" alt="" id="logo">
        <h1>Iniciar sesion</h1>
        <p>Sistema de Gestión de Documentos</p>
      </div>
      <form @submit="login">
        <div>
          <label for="username">Nombre de usuario</label>
          <input type="email" class="form-control" name="email" id="email" v-model="formData.email">
        </div>
        <div>
          <label for="password">Contraseña</label>
          <input type="password" class="form-control" name="password" id="password" v-model="formData.password">
        </div>
        <div class="btn-container">
          <button class="btn btn-primary" type="submit">Iniciar Sesión</button>
          <button type="button" class="line-btn" @click="register">Registrarse</button>
        </div>
      </form>
    </div>
  </main>
</template>

<script>
import { ref } from 'vue';
import Swal from 'sweetalert2';

const Toast = Swal.mixin({
  toast: true,
  position: 'top-end',
  showConfirmButton: false,
  timer: 3000,
  timerProgressBar: true,
  didOpen: (toast) => {
    toast.addEventListener('mouseenter', Swal.stopTimer)
    toast.addEventListener('mouseleave', Swal.resumeTimer)
  }
});
export default {
  setup() {
    return {
      title: 'Sistema de Gestión de Documentos'
    }
  },
  data() {
    return {
      formData: {
        email: '',
        password: ''
      }
    }
  },
  methods: {
    register() {
      this.$router.push('/registro');
    },
    login(event) {
      event.preventDefault();
      const users = JSON.parse(localStorage.getItem('users'));
      if (!users) {
        Toast.fire({
          icon: 'error',
          title: 'No hay usuarios registrados'
        });
        return;
      }
      console.log(users);
      const user = users.find(user => user.email === this.formData.email && user.password === this.formData.password);
      if (user) {
        localStorage.setItem('user', JSON.stringify(user));
        Toast.fire({
          icon: 'success',
          title: 'Inicio de sesión exitoso'
        });
        this.$router.push('/administrador');
      } else {
        Toast.fire({
          icon: 'error',
          title: 'Credenciales incorrectas'
        });
      }
    }
  }
}
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: Arial, Helvetica, sans-serif;
}

#logo {
  width: auto;
  height: 70px;
}

.header-logo {
  display: flex;
  align-items: center;
  gap: 1rem;
}
.line-btn {
  background-color: transparent;
  color: #666;
  border: none;
  cursor: pointer;
  transition: all 0.3s;
}

.line-btn:hover {
  color: #333;
  text-decoration: underline;
}
.btn-primary {
  background-color: #8fe697;
  color: #fff;
  font-size: 1rem;
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: all 0.3s;
}

input {
  width: 100%;
  padding: 0.5rem;
  margin: 0.5rem 0;
  border: 1px solid #ccc;
  border-radius: 5px;

}

.btn-primary:hover {
  background-color: #499e50;
}
form .btn-container {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 1rem;
}
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem;
  background-color: #f1f1f1;
}

.form-container {
  width: 100%;
  max-width: 400px;
  margin: 0 auto;
  padding: 1rem;
  border-radius: 5px;
  background-color: #fff;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}
.form-container form div {
  margin: 1rem 0;
}
main {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 90vh;
  background: url(../assets/uleam-acceso-principal.webp) no-repeat center center/cover;
}
.form-header h1 {
  font-size: 2rem;
  margin-bottom: 0.5rem;
}

.form-header p {
  font-size: 1rem;
  color: #666;

}
.form-header {
  text-align: center;
  margin-bottom: 1rem;
}
</style>

