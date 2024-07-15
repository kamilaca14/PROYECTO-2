<template>
  <header>
    <div class="header-logo">
      <img src="../assets/logo_ULEAM_2017_horizontal.png" alt="" id="logo">
      <h2>Registrarse</h2>
    </div>

  </header>
  <main>
    <div class="container">
      <form @submit.prevent="registrarse">
        <div class="img-container">
          <img src="../assets/logo_ULEAM_2017_horizontal.png" alt="logo" id="login">
        </div>
        <div class="text-center">
          <h2>Registrarse</h2>
          <p class="span-text">Sistema de documentos de la ULEAM</p>
        </div>
        <div>
          <label for="nombre">Nombre</label>
          <input
            type="text"
            class="form-control"
            v-model="formData.value.nombre"
          />
        </div>
        <div>
          <label for="apellido">Apellido</label>
          <input
            type="text"
            class="form-control"
            v-model="formData.value.apellido"
          />
        </div>
        <div>
          <label for="email">Nombre de usuario</label>
          <input
            type="text"
            class="form-control"
            v-model="formData.value.username"
          />
        </div>
        <div>
          <label for="email">Correo electrónico</label>
          <input
            type="email"
            class="form-control"
            v-model="formData.value.email"
          />
        </div>
        <div>
          <label for="password">Contraseña</label>
          <input
            type="password"
            class="form-control"
            v-model="formData.value.password"
          />
        </div>
        <div>
          <label for="confirmPassword">Confirmar contraseña</label>
          <input
            type="password"
            class="form-control"
            v-model="formData.value.confirmPassword"
          />
        </div>
        <div class="btn-container">
          <div>
            <button type="submit" class="btn btn-primary boton">Registrarse</button>
          </div>
          <div>
            <button type="button" class="line-btn" @click="login">Iniciar sesión</button>
          </div>
        </div>
      </form>
    </div>
  </main>
</template>

<script>
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
  data() {
    return {
      formData: {
        value: {
          nombre: "",
          apellido: "",
          email: "",
          password: "",
          confirmPassword: "",
          username: ""
        },
      },
    };
  },
  methods: {
    registrarse() {
      const { nombre, apellido, username, email, password, confirmPassword } = this.formData.value;
      if (!nombre || !apellido || !username || !email || !password || !confirmPassword) {
        Toast.fire({
          icon: 'error',
          text: 'Por favor, complete todos los campos',
        });
        return;
      }
      if (password !== confirmPassword) {
        Toast.fire({
          icon: 'error',
          text: 'Las contraseñas no coinciden',
        });
        return;
      }
      const users = JSON.parse(localStorage.getItem('users')) || [];
      const user = users.find((user) => user.email === email);
      if (user) {
        Toast.fire({
          icon: 'error',
          text: 'El correo electrónico ya está registrado',
        });
        return;
      }
      users.push({ nombre, apellido, username, email, password });
      localStorage.setItem('users', JSON.stringify(users));
      Toast.fire({
        icon: 'success',
        text: 'Usuario registrado correctamente',
      }).then(() => {
        this.$router.push('/');
      });
    },
    login() {
      this.$router.push("/");
    },
  },
};
</script>

<style scoped>
* {
  box-sizing: border-box;
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
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem;
  background-color: #f1f1f1;
}
.span-text {
  font-size: 14px;
  color: rgb(83, 83, 83);
  font-weight: bold;
  text-align: center;
}
.img-container {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

#login {
  width: 200px;
  height: auto;
  text-align: center;
}

.text-center {
  text-align: center;

}
.boton {
  background: #83d68e;
  color: white;
  border: none;
}
main {
  display: flex;
  justify-content: center;
  align-items: center;
  height: auto;
  padding: 10px 0;
  background-image: url(../assets/uleam-acceso-principal.webp);
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
}

.container {
  max-height: 800px;
  background: rgb(255, 255, 255);
  padding: 20px;
  border-radius: 10px;
  max-width: 600px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

input {
  width: 100%;
  padding: 10px;
  margin: 10px 0;
  border: 1px solid rgb(0, 0, 0, 0.1);
  border-radius: 5px;
}

.line-btn {
  color: rgb(53, 53, 53);
  background: none;
  border: none;
  font-size: 14px;
  cursor: pointer;
}

h2 {
  font-weight: bolder;
}

input {
  width: 100%;
  padding: 5px 10px;
  margin: 10px 0;
  border: 1px solid rgb(0, 0, 0, 0.1);
  border-radius: 5px;
}

.btn-container {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 1rem;
}
</style>
