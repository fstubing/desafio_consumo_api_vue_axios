<template>
  <h1 class="text-center my-5">RANDOM CHAT</h1>
  <div class="row m-5">
    <div class="col-4 d-flex justify-content-end">
      <div class="card" style="width: 18rem">
        <img :src="fotosDeLosUsuarios[0]" class="card-img-top" alt="..." />
        <div class="card-body">
          <h5 class="card-title">{{ nombresDeLosUsuarios[0] }}</h5>
          <form action="" @submit.prevent="agregarChat1">
            <input
              type="color"
              class="form-control form-control-color w-100 mt-3"
              id="exampleColorInput"
              value="{{ form1.colorChat }}"
              title="Choose your color"
              v-model="form1.colorChat"
            />
            <textarea
              class="form-control mt-3"
              id="floatingTextarea"
              rows="3"
              v-model="form1.mensaje"
            ></textarea>
            <button type="submit" class="btn btn-secondary w-100 mt-3">
              enviar
            </button>
          </form>
        </div>
      </div>
    </div>
    <div class="col-4">
      <div class="h-100 bg-warning-subtle chatContainer">
        <div
          v-for="(chat, index) in chats"
          :key="index"
          :class="chat.alineado ? 'izquierda' : 'derecha'"
        >
          <p class="mb-0 nombreChico">{{ chat.origenMsg }}</p>
          <span
            class="rounded-pill px-2"
            :style="{ backgroundColor: chat.colorChat }"
          >
            {{ chat.mensaje }}
          </span>
        </div>
      </div>
    </div>
    <div class="col-4">
      <div class="card" style="width: 18rem">
        <img :src="fotosDeLosUsuarios[1]" class="card-img-top" alt="..." />
        <div class="card-body">
          <h5 class="card-title">{{ nombresDeLosUsuarios[1] }}</h5>
          <form action="" @submit.prevent="agregarChat2">
            <input
              type="color"
              class="form-control form-control-color w-100 mt-3"
              id="exampleColorInput"
              value="{{ form2.colorChat }}"
              title="Choose your color"
              v-model="form2.colorChat"
            />
            <textarea
              class="form-control mt-3"
              id="floatingTextarea"
              rows="3"
              v-model="form2.mensaje"
            ></textarea>
            <button type="submit" class="btn btn-secondary w-100 mt-3">
              enviar
            </button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "App",
  components: {},
  data() {
    return {
      usuarios: [],
      chats: [],
      form1: {
        mensaje: "",
        origenMsg: "",
        colorChat: "rgb(255, 255, 255)",
        alineado: "",
      },
      form2: {
        mensaje: "",
        origenMsg: "",
        colorChat: "rgb(255, 255, 255)",
        alineado: "",
      },
      colorChatDerecho: "",
      colorChatIzquierdo: "",
    };
  },
  async mounted() {
    try {
      const url = "https://randomuser.me/api?results=2";
      const { data } = await axios.get(url);
      this.usuarios = data.results;
      console.log(this.usuarios[0]);
    } catch (error) {
      console.log(error);
    }
  },
  computed: {
    fotosDeLosUsuarios() {
      return this.usuarios.map((usuario) => usuario.picture.large);
    },
    nombresDeLosUsuarios() {
      return this.usuarios.map(
        (usuario) => `${usuario.name.first} ${usuario.name.last}`
      );
    },
  },
  methods: {
    agregarChat1() {
      this.form1.origenMsg = this.nombresDeLosUsuarios[0];
      this.form1.alineado = true;
      this.chats.push({ ...this.form1 });
      console.log(this.form1);
      this.form1 = {
        mensaje: "",
        origenMsg: "",
      };
    },

    agregarChat2() {
      this.form2.origenMsg = this.nombresDeLosUsuarios[1];
      this.form2.alineado = false;
      this.chats.push({ ...this.form2 });
      console.log(this.form2);
      this.form2 = {
        mensaje: "",
        origenMsg: "",
      };
    },
  },
};
</script>

<style>
.chatContainer {
  overflow-y: scroll;
  scrollbar-width: thin;
}
.nombreChico {
  font-size: smaller;
}

.derecha {
  margin: 5px;
  display: flex;
  flex-direction: column;
  align-items: end;
}

.izquierda {
  margin: 5px;
  display: flex;
  flex-direction: column;
  align-items: start;
}
</style>
