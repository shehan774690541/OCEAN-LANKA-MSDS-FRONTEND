<template>
  <div class="table-container">
    <!-- Page Header -->
    <p class="page-header">
      <b><span style="color: green;">O</span>
        <span style="color: aqua;">C</span>
        <span style="color: green;">E</span>
        <span style="color: red;">A</span>
        <span style="color: green;">N</span>&nbsp;
        <span style="color: green;">L</span>
        <span style="color: red;">A</span>
        <span style="color: green;">N</span>
        <span style="color: aqua;">K</span>
        <span style="color: green;">A</span></b> MSDS Listing System
    </p>

    <!-- earch Bar -->
    <div class="search-bar">
      <input type="text" v-model="inputSerch" v-on:keyup="searchValue()" placeholder="🔍Search ....">
      <select v-model="searchDepsValue" @change="searchDeps()">
        <option value="All">All</option>
        <option value="Dyeing"> Dyeing </option>
        <option value="Finishing"> Finishing </option>
        <option value="Knitting"> Knitting </option>
        <option value="Lab Dip"> Lab Dip </option>
        <option value="ETP"> ETP </option>
        <option value="Printing"> Printing </option>
        <option value="Testing Lab"> Testing Lab </option>
        <option value="Chemical W.H"> Chemical W.H </option>

      </select>
    </div>

    <!-- Data Table -->
    <table class="msds-table">
      <thead>
        <tr>
          <th></th>
          <th>Msds Name</th>
          <th>Departments</th>
          <th>Edit</th>
          <th>Delete</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="msdsData in msdsDatas" :key="i">
          <!-- <td>{{ i }}</td> -->
          <td> <input type="checkbox" v-model="msdsData.check"> </td>
          <td> <a :href="msdsData.fileContent" target="_blank" title="Click And Open The MSDS"
              style="text-decoration: none; color: blue">{{
                msdsData.name }}</a> </td>
          <td>
            <ul v-for="dep in msdsData.deps">
              <li>{{ dep }}</li>
            </ul>
          </td>
          <td style="width: 40px;"><button class="edit-btn" v-on:click="editMsdsData(msdsData.id)">Edit</button></td>
          <td style="width: 30px;"><button class="delete-btn"
              v-on:click="deleteIthem(msdsData.id, msdsData.name)">Delete</button></td>
        </tr>
      </tbody>
    </table>

    <!-- Create New MSDS buton -->
    <button class="new-buttons" v-on:click="this.popup = 'new'">✏️</button>

    <!-- Create New Popup -->
    <div class="popup" v-if="popup == 'new'">
      <div class="popup-viewer">
        <newMsds :indexNb="'0'" />
      </div>
      <div class="popup-close" v-on:click="this.popup = ''">❌</div>
    </div>

    <!-- Update Popup -->
    <div class="popup" v-if="popup == 'edit'">
      <div class="popup-viewer">
        <editMsds :indexNb="editId" />
      </div>
      <div class="popup-close" v-on:click="this.popup = ''">❌</div>
    </div>


  </div>
</template>
<script>
import newMsds from '@/components/NewMsds.vue'
import editMsds from '@/components/EditMsds.vue'

export default {
  data() {
    return {
      val: false,
      editId: 0,
      popup: '',
      inputSerch: '',
      searchDepsValue: 'All',
      msdsDatas: [],
      example: [
        {
          id: 1,
          check: false,
          name: 'Sodium Chloride (NaCl)',
          fileContent: 'https://en.wikipedia.org/wiki/Chemical_substance',
          deps: ["Dyeing"],
        },
        {
          id: 2,
          check: true,
          name: 'Sulfuric Acid (H₂SO₄)',
          fileContent: 'https://en.wikipedia.org/wiki/Chemical_substance',
          deps: ["Knitting", "ETP", "Dyeing"],
        },
        {
          id: 3,
          check: false,
          name: 'Ethanol (C₂H₅OH)',
          fileContent: 'https://en.wikipedia.org/wiki/Chemical_substance',
          deps: ["Knitting", "Printing"],
        },
        {
          id: 4,
          check: false,
          name: 'Ammonia (NH₃)',
          fileContent: 'https://en.wikipedia.org/wiki/Chemical_substance',
          deps: ["Knitting", "Finishing", "Dyeing", "Chemical W.H",],
        },
        {
          id: 5,
          check: false,
          name: 'Hydrogen Peroxide (H₂O₂)',
          fileContent: 'https://en.wikipedia.org/wiki/Chemical_substance',
          deps: ["Testing Lab", "Finishing", "Dyeing"],
        },
        {
          id: 8,
          check: true,
          name: 'Calcium Carbonate (CaCO₃)',
          fileContent: 'https://en.wikipedia.org/wiki/Chemical_substance',
          deps: ["Testing Lab", "Chemical W.H", "Lab Dip"],
        }
      ]
    }
  },
  mounted() {
    this.listTheData()
  },
  methods: {
    listTheData() {
      this.msdsDatas = this.example.sort((a, b) => a.name.localeCompare(b.name));
    },
    editMsdsData(i) {
      this.editId = i
      this.popup = 'edit'
    },
    searchValue() {
      // console.log(this.inputSerch)
      this.msdsDatas = this.example.filter(example => {
        return example.name.toLowerCase().includes(this.inputSerch.toLowerCase());
      });
    },
    searchDeps() {
      if (this.searchDepsValue === "All") {
        this.msdsDatas = this.example;  // Reset to original dataset if "All" is selected
      } else {
        this.msdsDatas = this.example.filter(item => item.deps.includes(this.searchDepsValue));
      }
    },
    deleteIthem(delId, ithem) {
      if (confirm(`Are You Shuwer Delete "${ithem}"`)) {
        console.log(`Ithem-${delId} Deleted!`)
      }
    }

  },
  components: {
    newMsds,
    editMsds
  }

}
</script>
<style>
.popup {
  width: 100%;
  height: 100vh;
  position: fixed;
  top: 0;
  z-index: 1000;
  backdrop-filter: blur(2.9px);
  display: flex;
  align-items: center;
  justify-content: center;
  /* mix-blend-mode: multiply; */
  transition: ease 1s;
}

.popup-viewer {
  width: 300px;
  height: 300px;
  padding: 0px 20px 40px 20px;
  background-color: #ececec;
  border: solid 1px;
  border-radius: 10px;
  align-items: center;
  justify-content: center;
}

.popup-close {
  position: fixed;
  background-color: #dee2e6;
  padding: 5px;
  border-radius: 100%;
  top: 10px;
  right: 30px;

}

.page-header {
  width: 100%;
  text-align: center;
  font-size: 2rem;
  margin: 0;
  padding: 0;
  text-decoration: underline;
}

.table-container {
  max-width: 100vw;
  height: 100vh;
  max-height: 80vh;
  overflow-y: auto;
  padding: 20px;
  background-color: #f8f9fa;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  border-radius: 10px;
  justify-content: center;
  align-items: center;
}

.msds-table {
  width: 100%;
  border-collapse: separate;
  border-spacing: 0 10px;
  font-family: 'Arial', sans-serif;
}

thead {
  background-color: #6c757d;
  color: #fff;
  position: sticky;
  top: 0;
  z-index: 1;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

thead th {
  padding: 12px;
  text-align: left;
}

tbody tr {
  background-color: #fff;
  transition: transform 0.2s ease;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
}

tbody tr:hover {
  transform: scale(1.006);
  box-shadow: 0px 0px 20px 3px rgba(131, 131, 131, 0.74);

}

tbody td {
  padding: 12px;
  border-bottom: 1px solid #dee2e6;
}

tbody tr:nth-child(even) {
  background-color: #f3f3f3;
}

.edit-btn,
.delete-btn {
  background-color: transparent;
  border: 2px solid transparent;
  color: #fff;
  padding: 6px 12px;
  border-radius: 4px;
  font-size: 0.875rem;
  cursor: pointer;
  transition: background-color 0.3s ease, color 0.3s ease;
}

.edit-btn {
  background-color: #28a745;
}

.delete-btn {
  background-color: #dc3545;
}

.edit-btn:hover {
  background-color: #218838;
}

.delete-btn:hover {
  background-color: #c82333;
}

td button {
  color: #fff;
  font-weight: bold;
}

.search-bar {
  width: 100%;
  display: flex;
  justify-content: end;
  margin: 10px 0px 2px 0px;
}

.search-bar input {
  padding: 2px;
  font-size: 1rem;
  text-align: end;
}

.new-buttons {
  position: fixed;
  right: 10px;
  bottom: 10px;
  border: solid 1px;
  border-radius: 100%;
  padding: 5px;
  font-size: 1.2rem;
}

.new-buttons:active,
.popup-close:active {
  box-shadow: rgb(0, 0, 0) 0px 0px 10px 2px;
  padding: 4.5px;
}
</style>
