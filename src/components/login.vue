<template>
  <div class="content">
    <div id="form">
      <h1>Faça o seu login</h1>
      <form @submit.prevent="login()">
        <label for-id="email" class="sr-only">Usuário</label>

        <input
          type="text"
          id="email"
          placeholder="Digite seu email de usuário"
          v-model="inputName"
        />

        <label for="inputPassword" class="sr-only">Senha</label>

        <input
          type="password"
          id="inputPassword"
          placeholder="Digite sua senha"
          v-model="inputPass"
        />
        <input type="submit" value="Login" />
      </form>
    </div>
  </div>
</template>
<script>
import router from "@/router";
export default {
  name: "LoginForm",
  data() {
    return {
      inputName: "",
      inputPass: "",
    };
  },

  methods: {
    login() {
      fetch("http://localhost:8000/api/login", {
        method: "POST",
        body: JSON.stringify({
          email: this.inputName,
          password: this.inputPass,
        }),
        headers: {
          "Content-Type": "application/json",
        },
      }).then((resp) => {
        if (resp.status !== 200) {
          alert("login incorreto");
          return;
        }
        router.push("users");
      });
    },
  },
};
</script>

 <style scoped>
* {
  margin: 0;
  padding: 0;
  border: none;
  text-decoration: none;
  box-sizing: border-box;
}

.content {
  display: flex;
  height: 100vh;
  align-items: center;
  justify-content: center;
  background: rgb(194, 189, 189) f;
}

#form {
  box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;
  padding: 5rem 10rem;
}

#form h1 {
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  margin-bottom: 2rem;
  font-size: 2.5rem;
  text-align: center;
  color: #0d114f;
}

#form form {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
  flex-direction: column;
  gap: 2rem;
}

#form form input:nth-child(2),
#form form input:nth-child(4) {
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  font-size: 16px;
  border: 0.2rem solid #a1b2cd;
  padding: 0 1.6rem;

  outline: none;
  width: 22rem;
  transition: 0.4s;
}

#form form input:nth-child(2):focus,
#form form input:nth-child(4):focus {
  border: 0.2rem solid #3485ff;
}

#form form input:nth-child(5) {
  background: #3485ff;
  color: #ffff;
  cursor: pointer;
  transition: 0.4s;
  font-weight: 700;
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  letter-spacing: 1px;
  font-size: 16px;
}

#form form input:nth-child(5):hover {
  filter: brightness(1.2);
}

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

input,
button {
  width: 100%;
  border-radius: 0.8rem;
  height: 5rem;
}
</style>
