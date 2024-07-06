<!-- eslint-disable vue/no-reserved-component-names -->
<template>
  <ion-page>
    <ion-content :fullscreen="true">
      <ion-header :translucent="true">
        <ion-title>Impostazioni</ion-title>
      </ion-header>
      <Form @submit="onSubmit" :validation-schema="schema">
        <ion-item>
          <TextInput
            name="username"
            id="username"
            type="text"
            label="Codice Utente"
            placeholder="codice utente"
            success-message=" "
            :value="username_input"
            maxlength="20"
          />
        </ion-item>

        <ion-item>
          <label for="sede">Sede di preparazione: </label>
          <select id="sede_preparazione" v-model="sede_selected">
            <option v-for="s in sedi" :key="s" :value="s.value">
              {{ s.text }}
            </option>
          </select>
          <div class="help-message">
            {{ errorMessage }}
          </div>
        </ion-item>
        <ion-item>
          <label for="sede">Modalità lettura Qrcode: &nbsp;&nbsp;</label>
          <select id="tipo_lettura" v-model="tipo_lettura_selected">
            <option v-for="s in tipi_lettura" :key="s" :value="s.value">
              {{ s.text }}
            </option>
          </select>
          <div class="help-message">
            {{ errorMessage }}
          </div>
        </ion-item>
        <ion-item
          ><span class="errore">{{ message }}</span>
          <input type="hidden" id="update" name="update" value=""
        /></ion-item>
        <ion-toolbar>
          <ion-button id="submit" type="submit">Login</ion-button>
        </ion-toolbar>
      </Form>
    </ion-content>
  </ion-page>
</template>

<script>
import { ref, onMounted } from "vue";
import axios from "axios";
import { Form } from "vee-validate";
import * as Yup from "yup";
//import TextInput from "../components/TextInput.vue";
import { IonContent, IonPage, IonToolbar, IonButton } from "@ionic/vue";
import TextInput from "@/components/TextInput.vue";
//import { useRouter } from "vue-router";

export default {
  name: "LoginPage",
  components: {
    // eslint-disable-next-line vue/no-reserved-component-names
    Form,
    IonContent,
    IonPage,
    IonToolbar,
    IonButton,
    // IonInput,
    TextInput,
  },
  data() {
    return {
      input: {
        username: "",
        password: "",
      },
      response: {},
    };
  },

  setup() {
    const message = ref("");
    const errorMessage = ref("");
    const sedi = ref([{}]);
    const tipi_lettura = ref([{}]);
    const username_input = ref("");
    const sede_selected = ref("");
    const tipo_lettura_selected = ref("");
    const obj = [
      {
        value: "F",
        text: "San Giovanni",
      },
      {
        value: "M",
        text: "Montecchia",
      },
    ];

    sedi.value = obj;
    const obj_tipo = [
      {
        value: "1",
        text: "Lettore integrato",
      },
      {
        value: "2",
        text: "Telecamera",
      },
    ];
    tipi_lettura.value = obj_tipo;
    // const router = useRouter();
    async function onSubmit(values) {
      console.log(values);
      if (document.getElementById("sede_preparazione").value == "") {
        errorMessage.value = "Selezionare una sede di preparazione";
        return;
      }
      if (document.getElementById("tipo_lettura").value == "") {
        errorMessage.value = "Selezionare una tipologia di lettura";
        return;
      }
      const url =
        "http://frauweb.frau.it/spedizioni/ajax/index.php?action=ajax&task=checkutente";

      const fd = new FormData();
      fd.append("username", values["username"]);
      fd.append("action", "ajax");
      fd.append("task", "checkutente");
      //    console.log(data,fd);
      const config = {
        mode: "no-cors",
        Accept: "application/json, text/plain, *",
      };

      await axios.post(url, fd, config).then((response) => {
        console.log(response.data);
        if (response.data.status == "200") {
          //   this.message = values["username"];

          message.value = values["username"];
          localStorage.username = values["username"];
          document.getElementById("update").value = "1";

          //    this.storage.set("data_login", currentDate);
          localStorage.tipo_lettura = document.getElementById("tipo_lettura").value;

          localStorage.sessione_corrente = "";
          localStorage.sede_preparazione = document.getElementById(
            "sede_preparazione"
          ).value;

          self.location.href = "/folder/login";
          //          routeRedirect.setAttribute("to", "/folder/etichetta");
          return "200";
        } else {
          message.value = response.data.message;
        }
      });
    }

    function onInvalidSubmit() {
      const submitBtn = document.querySelector(".submit-btn");
      submitBtn.classList.add("invalid");
      setTimeout(() => {
        submitBtn.classList.remove("invalid");
      }, 1000);
    }
    onMounted(() => {
      console.log("mounted");

      document.getElementById("update").value = "9";
      if (localStorage.username != undefined)
        username_input.value = localStorage.username;
      console.log(username_input);
      if (localStorage.sede_preparazione != undefined)
        sede_selected.value = localStorage.sede_preparazione;
      if (localStorage.tipo_lettura != undefined)
        tipo_lettura_selected.value = localStorage.tipo_lettura;
    });

    // Using yup to generate a validation schema
    // https://vee-validate.logaretm.com/v4/guide/validation#validation-schemas-with-yup
    const schema = Yup.object().shape({
      username: Yup.string().required(),
      //      sede: Yup.string().required(),
    });
    const initialValues = {
      username: username_input.value,
    };
    return {
      onSubmit,
      schema,
      onInvalidSubmit,
      message,
      errorMessage,
      sedi,
      username_input,
      tipi_lettura,
      sede_selected,
      tipo_lettura_selected,
    };
  },
  methods: {},
};
</script>

<style>
* {
  box-sizing: border-box;
}

:root {
  --primary-color: #0071fe;
  --error-color: #f23648;
  --error-bg-color: #fddfe2;
  --success-color: #21a67a;
  --success-bg-color: #e0eee4;
}

html,
body {
  width: 100%;
  height: 100%;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;

  width: 100%;
  height: 100%;
}

.errore {
  color: #f23648;
}

form {
  width: 100%;
  margin: 0px auto;
  padding-bottom: 60px;
}

#container {
  text-align: center;
  left: 0;
  right: 0;
  top: 3%;
  width: 100%;
  height: 300px;
}

#container strong {
  font-size: 20px;
  line-height: 26px;
}

#container p {
  font-size: 16px;
  line-height: 22px;

  color: #8c8c8c;

  margin: 0;
}

#container a {
  text-decoration: none;
}

input,
label {
  display: block;
  margin-top: 20px;
}
.inpu {
  background-color: #bfe0bf;
  width: 5em;
}
#data_evento {
  width: 50%;
}

input + span {
  display: block;
}

button {
  margin-top: 20px;
}

.help-message {
  color: red;
}
</style>
