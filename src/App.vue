<template>
  <ion-app>
    <ion-split-pane content-id="main-content">
      <ion-menu v-if="isLogin == '1'" content-id="main-content" type="overlay">
        <ion-content>
          <ion-list id="inbox-list">
            <ion-list-header>terminal</ion-list-header>
            <ion-note>Gestione spedizioni</ion-note>

            <ion-menu-toggle auto-hide="false" v-for="(p, i) in appPages" :key="i">
              <ion-item
                @click="selectedIndex = i"
                router-direction="root"
                :router-link="p.url"
                lines="none"
                detail="false"
                class="hydrated"
                :class="{ selected: selectedIndex === i }"
              >
                <ion-icon
                  aria-hidden="true"
                  slot="start"
                  :ios="p.iosIcon"
                  :md="p.mdIcon"
                ></ion-icon>
                <ion-label>{{ p.title }}</ion-label>
              </ion-item>
            </ion-menu-toggle>
          </ion-list>
          <ion-toolbar>
            <ion-item> <ion-button @Click="Logout()">Logout</ion-button></ion-item>
          </ion-toolbar>
        </ion-content>
      </ion-menu>

      <ion-router-outlet id="main-content"></ion-router-outlet>
    </ion-split-pane>
  </ion-app>
</template>

<script>
import { ref, onMounted } from "vue";
import { Storage } from "@ionic/storage";
import { useRouter } from "vue-router";
import {
  IonApp,
  IonContent,
  IonIcon,
  IonItem,
  IonToolbar,
  IonLabel,
  IonList,
  IonListHeader,
  IonMenu,
  IonMenuToggle,
  IonNote,
  IonRouterOutlet,
  IonSplitPane,
} from "@ionic/vue";
import {
  archiveOutline,
  archiveSharp,
  heartOutline,
  heartSharp,
  mailOutline,
  mailSharp,
  paperPlaneOutline,
  paperPlaneSharp,
} from "ionicons/icons";
export default {
  data() {
    return {
      input: {
        username: "",
        password: "",
      },
      response: {},
      message: "",
    };
  },
  components: {
    IonApp,
    IonContent,
    IonIcon,
    IonItem,
    IonToolbar,
    IonLabel,
    IonList,
    IonListHeader,
    IonMenu,
    IonMenuToggle,
    IonNote,
    IonRouterOutlet,
    IonSplitPane,
  },
  methods: {
    Logout() {
      this.storage.set("data_login", "");
      this.isLogin = "0";
      self.location.href = "/folder/login";
      //       this.router.push({ path: '/folder/login' })
    },
  },
  setup() {
    const router = useRouter();
    const authenticated = ref(true);
    const isLogin = ref("0");
    const selectedIndex = ref(0);
    const appPages = ref([
      {
        title: "Gestione colli",
        url: "/folder/etichetta",
      },
      {
        title: "Sessioni aperte",
        url: "/folder/sessioni",
        //       iosIcon: paperPlaneOutline,
        //       mdIcon: paperPlaneSharp,
      },
      {
        title: "DDT emesse",
        url: "/folder/Favorites",
        /*        iosIcon: heartOutline,
        mdIcon: heartSharp,
 */
      },
      {
        title: "Impostazioni",
        url: "/folder/impostazioni",
        /*        iosIcon: archiveOutline,
        mdIcon: archiveSharp,
 */
      },
      /*      {
        title: "TestForm",
        url: "/folder/testform",
         
      }, */
    ]);

    const storage = new Storage({
      name: "__mydb",
    });
    storage.create();
    onMounted(() => {
      /*       const queryString = window.location.search;
      const urlParams = new URLSearchParams(queryString);
      isLogin.value = "0";
      if (urlParams.has("update") == true) {
        isLogin.value = urlParams.get("update");
      }
      if (isLogin.value == "1") {
        console.log("isLogin", isLogin.value);
        router.push({ path: "/folder/etichetta", query: { isLogin: "1" } });
      }
      if (isLogin.value == "9") {
        isLogin.value = "";
        return;
      }
 */ //    if (isLogin.value != "1") {
      checkLogin(router);
      console.log("post" + isLogin.value);
      //      }
    });
    const checkLogin = () => {
      const data = localStorage.data_login;
      const date = new Date();
      const cDate = date.toJSON();
      const currentDate = cDate.slice(0, 10);

      if (data == currentDate) {
        isLogin.value = "1";

        //alert('App ' + data + ' ' + currentDate)
      } else {
        isLogin.value = "0";
        router.push({ path: "/folder/login" });
      }
    };

    return {
      isLogin,
      selectedIndex,
      appPages,
      authenticated,
      storage,
      router,
    };
  },
};
</script>

<style scoped>
ion-menu ion-content {
  --background: var(--ion-item-background, var(--ion-background-color, #511717));
}

ion-menu.md ion-content {
  --padding-start: 8px;
  --padding-end: 8px;
  --padding-top: 20px;
  --padding-bottom: 20px;
}

ion-menu.md ion-list {
  padding: 20px 0;
}

ion-menu.md ion-note {
  margin-bottom: 30px;
}

ion-menu.md ion-list-header,
ion-menu.md ion-note {
  padding-left: 10px;
}

ion-menu.md ion-list#inbox-list {
  border-bottom: 1px solid var(--ion-color-step-150, #d7d8da);
}

ion-menu.md ion-list#inbox-list ion-list-header {
  font-size: 22px;
  font-weight: 600;

  min-height: 20px;
}

ion-menu.md ion-list#labels-list ion-list-header {
  font-size: 16px;

  margin-bottom: 18px;

  color: #757575;

  min-height: 26px;
}

ion-menu.md ion-item {
  --padding-start: 10px;
  --padding-end: 10px;
  border-radius: 4px;
}

ion-menu.md ion-item.selected {
  --background: rgba(var(--ion-color-primary-rgb), 0.14);
}

ion-menu.md ion-item.selected ion-icon {
  color: var(--ion-color-primary);
}

ion-menu.md ion-item ion-icon {
  color: #616e7e;
}

ion-menu.md ion-item ion-label {
  font-weight: 500;
}

ion-menu.ios ion-content {
  --padding-bottom: 20px;
}

ion-menu.ios ion-list {
  padding: 20px 0 0 0;
}

ion-menu.ios ion-note {
  line-height: 24px;
  margin-bottom: 20px;
}

ion-menu.ios ion-item {
  --padding-start: 16px;
  --padding-end: 16px;
  --min-height: 50px;
}

ion-menu.ios ion-item.selected ion-icon {
  color: var(--ion-color-primary);
}

ion-menu.ios ion-item ion-icon {
  font-size: 24px;
  color: #73849a;
}

ion-menu.ios ion-list#labels-list ion-list-header {
  margin-bottom: 8px;
}

ion-menu.ios ion-list-header,
ion-menu.ios ion-note {
  padding-left: 16px;
  padding-right: 16px;
}

ion-menu.ios ion-note {
  margin-bottom: 8px;
}

ion-note {
  display: inline-block;
  font-size: 16px;

  color: var(--ion-color-medium-shade);
}

ion-item.selected {
  --color: var(--ion-color-primary);
}
</style>
