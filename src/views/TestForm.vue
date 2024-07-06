<template>
  <ion-page>
    <ion-content :fullscreen="true">
      <ion-header :translucent="true">
        <ion-title>Impostazioni</ion-title>
      </ion-header>
      <Form
        @submit="submitForm"
        :validation-schema="schema"
        :initialValues="initialValues"
      >
        <ion-item>
          <label for="username">Username:</label>
          <ion-input
            name="username"
            id="username"
            type="text"
            label="Codice Utente"
            placeholder="codice utente"
            v-model="form.username"
            :value="form.username"
            required
          />
          <div class="help-message">
            {{ errorMessage }}
          </div>
        </ion-item>
        <ion-item>
          <label for="password">Password:</label>
          <ion-input
            class="input"
            id="password"
            type="password"
            v-model="form.password"
            required
          />
        </ion-item>
        <ion-item>
          <label for="name">Name:</label>
          <input
            id="name"
            type="text"
            v-model="form.name"
            v-bind="nameAttrs"
            :value="form.name"
          />
        </ion-item>
        <ion-item>
          <label for="email">Email:</label>
          <input id="email" type="email" v-model="form.email" required />
        </ion-item>
        <ion-item>
          <label for="tipo_collegamento">Tipo Collegamento:</label>
          <select id="tipo_collegamento" v-model="form.tipo_collegamento" required>
            <option value="1">Diretto</option>
            <option value="2">Indiretto</option>
          </select>
        </ion-item>
        <ion-toolbar><ion-button type="submit">Submit</ion-button></ion-toolbar>
      </Form>
    </ion-content>
  </ion-page>
</template>

<script>
import { onMounted, ref } from "vue";
import { Form, useForm, useField } from "vee-validate";
import { IonContent, IonPage, IonToolbar, IonButton } from "@ionic/vue";
import * as Yup from "yup";
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
  },

  setup() {
    const { meta, defineField } = useForm();
    const { values } = useForm();
    const [name, nameAttrs] = defineField("name");
    const { handleSubmit } = useForm();
    const { errorMessage } = useField("username");
    const form = ref({
      username: "vvvvvvvvvvvvvvvv",
      password: "",
      email: "",
      tipo_collegamento: "1",
      name: "xxxxxxxxxxxxxxxx",
      nameAttrs: "",
    });
    onMounted(() => {
      name.value = "John";
    });
    const schema = Yup.object().shape({
      username: Yup.string().required(),
      email: Yup.string().required().email(),
      //      sede: Yup.string().required(),
    });
    const initialValues = {
      name: "",
      email: "bbbbbbb",
      title: "",
      review: "",
      rating: "",
      date: new Date(),
      wouldRecommend: false,
      product: "",
    };
    const submitForm = handleSubmit(() => {
      // Handle form submission logic here
      console.log(values);
      console.log("Form submitted:", form.value.name);
    });
    return {
      schema,
      submitForm,
      form,
      errorMessage,
      values,
    };
  },
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

.input {
  background-color: #bfe0bf;
  height: 2em;
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
