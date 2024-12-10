<template>
<div>
  <div><h3>Kullanıcı Bilgileri</h3></div>
  <div id="main1">
    <table>
      <thead> 
        <tr class="border1">
          <th class="id"></th>
          <th class="ad">Ad</th>
          <th class="email">E-posta</th>
          <th class="islem">İşlem</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="bilgi in info" :key="bilgi.id" class="border2">
          <td class="bilgi">{{ bilgi.id }}</td>
          <td class="name">{{ bilgi.name }}</td>
          <td class="eposta">{{ bilgi.email }}</td>
          <td>
            <button class="buton1" @click="duzenle(bilgi)">Düzenle</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>

    <div v-if="selectedUser" id="main2">
      <h3>Kullanıcı Bilgilerini Düzenle</h3>
      <form @submit.prevent="guncelle">
        <div id="main3">
          <div class="value1">
            <div class="a1">
              <label class="a6" for="name">Ad:</label>
              <input type="text" maxlength="20" v-model="selectedUser.name" class="value22"/> 
            </div>
            <div class="a2">
              <label class="a5" for="email">E-posta:</label>
              <input type="email" maxlength="23" v-model="selectedUser.email" class="value22" />
            </div>
          <div id="abc"><button class="buton2" type="submit">Güncelle</button></div>
          </div>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import "@/pages/index.css"

export default {
  async asyncData() {
    try {
      const response = await axios.get("https://jsonplaceholder.typicode.com/users");
      return {
        info: response.data,
      };
    } catch (error) {
      console.error("Bir hata oluştu", error);
      return {
        info: [],
      };
    }
  },

  data() {
    return {
      selectedUser: null, 
    };
  },

  methods: {
    duzenle(user) {
      this.selectedUser = { ...user }; 
    },

    async guncelle() {
      try {
        const response = await axios.put(
          `https://jsonplaceholder.typicode.com/users/${this.selectedUser.id}`,
          this.selectedUser
        );
        const index = this.info.findIndex(user => user.id === this.selectedUser.id);
        if (index !== -1) {
          this.$set(this.info, index, { ...this.selectedUser });  
        }

        alert("Kullanıcı başarıyla güncellendi!");
      } catch (error) {
        console.error("Güncelleme işlemi sırasında hata oluştu:", error);
        alert("Bir hata oluştu.");
      }
    },
  },
};
</script>
