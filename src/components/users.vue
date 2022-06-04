<template>
  <div>
    <div class="content">
      <header>
        <nav>
          <h1>Gerenciamento de usuários</h1>
        </nav>
      </header>
      <section class="sectionTable">
        <div><h1>Usuários</h1></div>
        <div>
          <button class="createUser" v-on:click="createModal = true">
            Criar novo
          </button>
        </div>

        <div class="divider"></div>

        <table>
          <tr>
            <th>Nome</th>
            <th>E-mail</th>
            <th>Senha</th>
          </tr>
          <tr v-for="user in users" v-bind:key="user.id">
            <td>{{ user.name }}</td>
            <td>{{ user.email }}</td>
            <td>{{ user.password }}</td>
            <td>
              <button class="edit" @click="showEditModal(user)">Editar</button>
            </td>
            <td>
              <button class="delete" @click="showDeleteModal(user.id)">
                Remover
              </button>
            </td>
          </tr>
        </table>
      </section>
    </div>
    <!--modals-->

    <div class="modal-wrapper change" v-if="editModal.show">
      <div class="modal">
        <h2>Editar cliente</h2>
        <p>Tem certeza que deseja editar esse cliente?</p>

        <form @submit.prevent="editUser()">
          <label class="sr-only" for-id="name">Nome do usuario</label>
          <input type="text" id="name" placeholder="Nome" v-model="edit_name" />

          <label class="sr-only" for-name="email">Email do usuario</label>
          <input
            type="email"
            name="email"
            placeholder="Email"
            v-model="edit_email"
          />

          <div class="buttons">
            <div v-on:click="editModal.show = false">Cancelar</div>
            <button type="submit">Sim, editar</button>
          </div>
        </form>
      </div>
    </div>

    <div class="modal-wrapper excluir" v-if="deleteModal.show">
      <div class="modal">
        <h2>Excluir cliente</h2>
        <p>Tem certeza que você deseja excluir este cliente?</p>

        <form @submit.prevent="deleteUser()">
          <label class="sr-only" for-name="password"
            >Digite sua senha de admin</label
          >
          <input
            type="password"
            name="password"
            placeholder="Digite sua senha de admin"
            v-model="delete_user"
          />

          <div class="buttons">
            <div v-on:click="deleteModal.show = false">Cancelar</div>
            <button type="submit">Sim, excluir</button>
          </div>
        </form>
      </div>
    </div>

    <div class="modal-wrapper create" v-if="createModal">
      <div class="modal">
        <h2>Criar cliente</h2>

        <form @submit.prevent="submit()">
          <label class="sr-only" for-name="name_create">Nome do usuario</label>
          <input
            type="text"
            name="name_create"
            placeholder="Ensira o nome do usuário"
            v-model="input_name"
          />

          <label class="sr-only" for-name="email_create"
            >Email do usuario</label
          >
          <input
            type="email"
            name="email_create"
            placeholder="Ensira o email do usuário"
            v-model="input_email"
          />

          <label class="sr-only" for-name="password_create"
            >Senha do usuario</label
          >
          <input
            type="text"
            name="password_create"
            placeholder="Ensira a senha do usuário"
            v-model="input_password"
          />

          <div class="buttons">
            <div v-on:click="createModal = false">Cancelar</div>
            <button type="submit">Sim, criar</button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
/*code of delete and editar*/

export default {
  data() {
    return {
      users: [],
      input_name: "",
      input_email: "",
      input_password: "",
      edit_name: "",
      edit_email: "",
      delete_user: "",
      editModal: {
        show: false,
        id: 0,
      },
      createModal: false,
      deleteModal: { id: 0, show: false },
    };
  },

  name: "User",

  methods: {
    showEditModal(user) {
      this.edit_name = user.name;
      this.edit_email = user.email;
      this.editModal.id = user.id;
      this.editModal.show = true;
    },

    editUser() {
      fetch("http://localhost:8000/api/users/" + this.editModal.id, {
        method: "PUT",
        body: JSON.stringify({
          name: this.edit_name,
          email: this.edit_email,
        }),
        headers: {
          "Content-Type": "application/json",
        },
      })
        .then(() => {
          this.getUsers();
        })
        .finally(() => {
          this.editModal.show = false;
        });
    },

    showDeleteModal(id) {
      this.deleteModal.show = true;
      this.deleteModal.id = id;
    },
    deleteUser() {
      fetch("http://localhost:8000/api/users/" + this.deleteModal.id, {
        method: "DELETE",
        body: JSON.stringify({
          password: this.delete_user,
        }),
        headers: {
          "Content-Type": "application/json",
        },
      })
        .then(() => {
          this.getUsers();
        })
        .finally(() => {
          this.deleteModal.show = false;
        });
    },
    getUsers() {
      fetch("http://localhost:8000/api/users")
        .then((response) => response.json())
        .then((r) => {
          this.users = r;
        });
    },
    submit() {
      const payload = {
        name: this.input_name,
        email: this.input_email,
        password: this.input_password,
      };

      console.log(payload);

      const requirements = {
        method: "POST",
        body: JSON.stringify(payload),
        headers: {
          "Content-type": "application/json",
        },
      };

      fetch("http://localhost:8000/api/users", requirements)
        .then((response) => response.json())
        .then((resp) => {
          this.getUsers();
        })
        .finally(() => {
          this.createModal = false;
        });
    },
  },

  created() {
    this.getUsers();
  },
};
</script>

<style >
.sr-only {
  position: absolute;
  height: 1px;
  width: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip-path: react(0, 0, 0, 0);
  white-space: nowrap;
  border-width: 0;
}

.divider {
  height: 4px;
  width: 100%;
  background: #0d114f;
  margin-top: 2rem;
  margin-bottom: 2rem;
}

* {
  margin: 0;
  padding: 0;
  border: none;

  box-sizing: border-box;
}

.content {
  height: 100%;
  width: 100%;
}

nav {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 5rem;

  padding-left: 1.5rem;
}

nav h1 {
  margin-top: 2rem;
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  margin-bottom: 2rem;
  font-size: 2.5rem;
  text-align: center;
  color: #0d114f;
}

.sectionTable {
  height: 100%;
  padding: 2rem 10rem;
  display: grid;
}

.sectionTable h1 {
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  margin-bottom: 1rem;
  font-size: 1.8rem;

  color: #0d114f;
}

.sectionTable .createUser {
  padding: 0.5rem 1rem;
  border-radius: 6px;
  border: 1px solid #3485ff;
  background: white;
  cursor: pointer;
  color: #3485ff;
  transition: 0.4s;
}

.sectionTable .createUser:hover {
  filter: brightness(1.2);
}

.sectionTable table {
  border-collapse: collapse;
}

.sectionTable table th {
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  font-weight: bold;
  font-size: 1rem;

  text-align: left;
  padding-bottom: 0.5rem;
  padding-top: 0.5rem;
}

.sectionTable table td {
  border-bottom: 1px solid black;
  text-align: left;
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  font-weight: 400;
  font-size: 1rem;
}

.sectionTable table td button.edit {
  padding: 0.5rem 0.8rem;
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  font-weight: 400;
  border-radius: 6px;
  font-size: 1rem;
  background: #4d5e77;
  color: white;
  transition: 0.4s;
  cursor: pointer;
  margin-bottom: 10px;
  margin-top: 10px;
}

.sectionTable table td button.delete {
  padding: 0.5rem 0.8rem;
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  font-weight: 400;
  border-radius: 6px;
  font-size: 1rem;
  background: #e83f5b;
  color: white;
  transition: 0.4s;
  cursor: pointer;
  margin-top: 10px;
  margin-bottom: 10px;
}

/*modal style*/

.modal-wrapper {
  position: fixed;
  width: 100vw;
  height: 100vh;
  top: 0;
  left: 0;
  display: flex;

  background: rgba(10, 27, 55, 0.8);
}

.modal-wrapper.change .modal {
  padding: 4rem;
}

.modal-wrapper .modal {
  width: min(59rem, 90%);
  background: #ffff;
  margin: auto;
  border-radius: 0.8rem;

  text-align: center;
  padding: 4rem;
}

.modal-wrapper .modal h2 {
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  font-weight: 700;
  font-size: 2.4rem;
  margin-bottom: 1.2rem;

  color: #0d114f;
}

.modal-wrapper .modal p {
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  font-weight: 400;
  font-size: 1.6rem;
  color: #4d5e77;

  margin-bottom: 2.4rem;
}

.modal-wrapper .modal form input {
  background: #ffff;

  border: 0.2rem solid #3485ff;
  margin-bottom: 4rem;
  width: min(70%, 30.2rem);
  padding: 1.3rem 1.6rem;

  outline: none;
}

.modal-wrapper .modal .buttons {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.8rem;
}

.modal-wrapper .modal .buttons button {
  padding: 0.81rem 2rem;
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  font-weight: 400;
  border-radius: 6px;
  font-size: 1rem;
  background: #e83f5b;
  color: white;
  transition: 0.4s;
  cursor: pointer;
}

.modal-wrapper .modal .buttons button:hover {
  filter: brightness(1.2);
}

.modal-wrapper .modal .buttons div {
  margin-right: 2rem;
  background: #e5eaf1;
  padding: 0.81rem 2rem;
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  font-weight: 400;
  border-radius: 6px;
  font-size: 1rem;
  cursor: pointer;
  transition: 0.4s;
}

.modal-wrapper .modal .buttons div:hover {
  filter: brightness(0.8);
}

.modal-wrapper.change .modal form input,
.modal-wrapper.create .modal form input {
  margin-bottom: 2rem;
}

.modal-wrapper.change .modal .buttons button {
  background: #4d5e77;
}

.modal-wrapper.create .modal .buttons button {
  background: #51ac6c;
}
</style>
