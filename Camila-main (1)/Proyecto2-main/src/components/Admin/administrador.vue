<template>
  <header>
    <div class="logo-container">
      <img src="../../assets/logo_ULEAM_2017_horizontal.png" id="logo" alt="" />
      <h2>{{ subtitle }}</h2>
    </div>
    <h3 class="color-gray">
      Usuario: {{ currentUser.nombre }} {{ currentUser.apellido }}
    </h3>
    <div class="header-btn-container">
      <button class="btn btn-primary" @click="createDoc">
        Crear Documento
      </button>
      <button class="btn btn-danger" @click="logout">Cerrar Sesion</button>
    </div>
  </header>
  <main>
    <div class="main-text">
      <h1>Administrador de Documentos</h1>
      <p>Bienvenido al sistema de administración de documentos de la ULEAM</p>
    </div>
    <div class="table-container">
      <table class="table">
        <thead>
          <tr>
            <th>ID</th>
            <th>Nombre</th>
            <th>Descripcion</th>
            <th>Fecha de Creación</th>
            <th>Usuario</th>
            <th>Acciones</th>
          </tr>
        </thead>
        <tbody v-if="hasLocalDocs">
          <tr v-for="doc in localDocuments" :key="doc.id">
            <td>{{ doc.id }}</td>
            <td>{{ doc.name }}</td>
            <td>{{ doc.description }}</td>
            <td>{{ doc.date }}</td>
            <td>{{ doc.user.nombre }} {{ doc.user.apellido }}</td>
            <td class="btn-row-container">
              <button
                class="btn-blue"
                title="Descargar Archivo"
                @click="downloadDoc(doc.id)"
              >
                <i class="bi bi-download"></i>
              </button>
              <button
                class="btn btn-primary"
                title="Editar documento"
                @click="editDoc(doc.id)"
              >
                <i class="bi bi-pencil-square"></i>
              </button>
              <button
                class="btn btn-danger"
                title="Eliminar documento"
                @click="deleteDoc(doc.id)"
              >
                <i class="bi bi-trash"></i>
              </button>
            </td>
          </tr>
        </tbody>
        <tbody v-else>
          <tr>
            <td colspan="6" class="no-docs">
              <div class="card">
                <h3>No hay documentos</h3>
                <button class="btn-primary" @click="createDoc">
                  Crear Documento
                </button>
              </div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </main>
</template>
<script>
import Swal from "sweetalert2";

const Toast = Swal.mixin({
  toast: true,
  position: "top-end",
  showConfirmButton: false,
  timer: 3000,
  timerProgressBar: true,
  didOpen: (toast) => {
    toast.addEventListener("mouseenter", Swal.stopTimer);
    toast.addEventListener("mouseleave", Swal.resumeTimer);
  },
});
export default {
  mounted() {
    const user = localStorage.getItem("user");
    if (!user) {
      this.$router.push("/login");
    }
    const localDocs = JSON.parse(localStorage.getItem("documentos"));
    console.log(localDocs);
    if (localDocs.length > 0 && localDocs !== null) {
      this.hasLocalDocs = true;
      this.localDocuments = localDocs;
    }
  },
  setup() {
    return {
      title: "Administrador",
      subtitle: "FACCI Documentos",
    };
  },
  data() {
    return {
      currentUser: JSON.parse(localStorage.getItem("user")),
      hasLocalDocs: false,
      localDocuments: [],
      formData: {
        values: {
          email: "",
          password: "",
        },
      },
    };
  },
  methods: {
    logout() {
      localStorage.removeItem("user");
      this.$router.push("/");
    },
    deleteDoc(id) {
      Swal.fire({
        title: "Eliminar Documento",
        text: "¿Estas seguro de eliminar este documento?",
        showCancelButton: true,
        confirmButtonText: "Eliminar",
        cancelButtonText: "Cancelar",
      }).then((result) => {
        if (result.isConfirmed) {
          const localDocs = JSON.parse(localStorage.getItem("documentos"));
          const newDocs = localDocs.filter((doc) => doc.id !== id);
          if (newDocs.length === 0) {
            this.hasLocalDocs = false;
            localStorage.removeItem("documentos");
          } else {
            localStorage.setItem("documentos", JSON.stringify(newDocs));
          }
          localStorage.setItem("documentos", JSON.stringify(newDocs));
          this.localDocuments = newDocs;
        }
      });
    },
    downloadDoc(id) {
      Toast.fire({
        icon: "info",
        title: "Descargando documento...",
      }).then(() => {
        setTimeout(() => {
          Toast.fire({
            icon: "success",
            title: "Documento descargado",
          });
        }, 2000);
      });
    },
    editDoc(id) {
      Swal.fire({
        title: "Editar Documento",
        html: `
          <input type="text" class="swal2-input"  name="title" placeholder="Nombre del documento" id="title">
          <input type="file" class="swal2-file" id="doc-file" disabled>
          <input class="swal2-input" placeholder="Descripcion del documento" name="desciption" id="desciption"></input>
        `,
        showCancelButton: true,
        confirmButtonText: "Guardar",
        cancelButtonText: "Cancelar",
        preConfirm: () => {
          const docName = document.getElementById("title").value;
          const docDesc = document.getElementById("desciption").value;
        },
      }).then((result) => {
        if (result.isConfirmed) {
          const docName = document.getElementById("title").value;
          const docDesc = document.getElementById("desciption").value;
          if (!docName || !docDesc) {
            Toast.fire({
              icon: "error",
              title: "Por favor, complete todos los campos",
            });
            return;
          }
          const localDocs = JSON.parse(localStorage.getItem("documentos"));
          const docIndex = localDocs.findIndex((doc) => doc.id === id);
          localDocs[docIndex].name = docName;
          localDocs[docIndex].description = docDesc;
          localStorage.setItem("documentos", JSON.stringify(localDocs));
          this.localDocuments = localDocs;
          Toast.fire({
            icon: "success",
            title: "Documento actualizado",
          });
        }
      });
    },
    createDoc() {
      Swal.fire({
        title: "Crear Documento",
        html: `
          <input type="text" class="swal2-input"  name="title" placeholder="Nombre del documento" id="title">
          <input type="file" class="swal2-file" id="doc-file" disabled>
          <input class="swal2-input" placeholder="Descripcion del documento" name="desciption" id="desciption"></input>
        `,
        showCancelButton: true,
        confirmButtonText: "Crear",
        cancelButtonText: "Cancelar",
        preConfirm: () => {
          const docName = document.getElementById("title").value;
          const docDesc = document.getElementById("desciption").value;
        },
      }).then((result) => {
        if (result.isConfirmed) {
          const docName = document.getElementById("title").value;
          const docDesc = document.getElementById("desciption").value;
          const localDocs =
            JSON.parse(localStorage.getItem("documentos")) || [];
          const doc = {
            id: localDocs.length + 1,
            name: docName,
            description: docDesc,
            user: this.currentUser,
            date: new Date().toLocaleDateString(),
          };
          localDocs.push(doc);
          localStorage.setItem("documentos", JSON.stringify(localDocs));
          this.hasLocalDocs = true;
          this.localDocuments = localDocs;
        }
      });
    },
  },
};
</script>

<style scoped>
.logo-container {
  display: flex;
  gap: 1rem;
  align-items: center;
}
.color-gray {
  color: #535353;
}
header {
  background-color: #f8f9fa;
  padding: 1rem;
  text-align: center;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.header-btn-container {
  display: flex;
  gap: 1rem;
}
.no-docs {
  text-align: center;
  padding: 1rem;
  font-size: 1.5rem;
  color: #666;
}
header h2 {
  font-size: 1.5rem;
  color: #666;
}

.btn-danger {
  background-color: #ff7167;
  color: #fff;
  font-size: 1rem;
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: all 0.3s;
}
.btn-danger:hover {
  background-color: #ff3d3d;
}

.btn-blue {
  background-color: #007bff;
  color: #fff;
  font-size: 1rem;
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: all 0.3s;
}
.btn-blue:hover {
  background-color: #0056b3;
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
#logo {
  width: 200px;
  height: auto;
}

main {
  padding: 1rem;
  min-height: 100vh;
  overflow-x: auto;
  display: flex;
  justify-self: center;
  align-items: center;
  flex-direction: column;
  background: url(../../assets/uleam-acceso-principal.webp) no-repeat center
    center/cover;
}
th,
td {
  padding: 0.5rem;
  text-align: center;
}
thead {
  background-color: #8fe697;
  color: #fff;
}
th {
  padding: 20px 10px;
}
.table-container {
  width: 100%;
  overflow-x: auto;
  display: flex;
  justify-self: center;
  align-items: center;
}
table {
  width: 100%;
  border-collapse: collapse;
  background-color: #fff;
}
.btn-row-container {
  display: flex;
  gap: 1rem;
  justify-content: center;
}
.card {
  padding: 1rem;
  border-radius: 5px;
  border: 3px dashed #ccc;
  background-color: #fff;
}
.main-text {
  text-align: center;
  color: #fff;
  margin: 1rem;
}
.main-text h1 {
  font-size: 2rem;
}
.main-text p {
  font-size: 1rem;
}
</style>
