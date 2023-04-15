<template>
  <div class="btn-container">
    <button @click="addRandomContact">Añadir contacto aleatorio</button>
    <button @click="sortByName">Ordenar por nombre</button>
    <button @click="sortByPopularity">Ordenar por popularidad</button>
    <table>
      <thead>
        <tr>
          <th>Foto</th>
          <th>Nombre</th>
          <th>Popularidad</th>
          <th>Ganó un Oscar</th>
          <th>Ganó un Emmy</th>
          <th>Acciones</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="contact in contacts" :key="contact.id">
          <td><img :src="contact.pictureUrl" alt=""></td>
          <td>{{ contact.name }}</td>
          <td>{{ contact.popularity }}</td>
          <td v-if="contact.wonOscar">🏆</td>
          <td v-else></td>
          <td v-if="contact.wonEmmy">🏆</td>
          <td v-else></td>
          <td><button @click="deleteContact(contact.id)">Eliminar</button></td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import contacts from "./contacts.json";
import { ref } from "vue";

export default {
  name: "App",
  setup() {
    const contactsRef = ref(contacts.slice(0, 5));

    function addRandomContact() {
      const remainingContacts = contacts.filter(
        (contact) => !contactsRef.value.includes(contact)
      );
      const randomIndex = Math.floor(Math.random() * remainingContacts.length);
      contactsRef.value.push(remainingContacts[randomIndex]);
    }

    function sortByName() {
      contactsRef.value.sort((a, b) => a.name.localeCompare(b.name));
    }

    function sortByPopularity() {
      contactsRef.value.sort((a, b) => b.popularity - a.popularity);
    }

    function deleteContact(id) {
      const index = contactsRef.value.findIndex((contact) => contact.id === id);
      if (index !== -1) {
        contactsRef.value.splice(index, 1);
      }
    }

    return {
      contacts: contactsRef,
      addRandomContact,
      sortByName,
      sortByPopularity,
      deleteContact,
    };
  },
};
</script>



<style>
/* Ajustes generales de la tabla */
button {
  background-color: #2980b9;
  color: #fff;
  border: none;
  border-radius: 5px;
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #1a5276;
}

button.delete {
  background-color: #e74c3c;
  margin-left: 10px;
}

button.delete:hover {
  background-color: #c0392b;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

th,
td {
  text-align: left;
  padding: 8px;
  border-bottom: 1px solid #ddd;
}

th {
  background-color: #2980b9;
  color: #fff;
}

tr:nth-child(even) {
  background-color: #f2f2f2;
}

tr:hover {
  background-color: #e0e0e0;
}

img {
  width: 120px;
  height: 120px;
  object-fit: cover;
  border-radius: 50%;
}

</style>