<template>
<div class="px-4 mx-4 min-h-screen border">
  <label for="upload">
    Upload JSON
    <input type="file" id="upload" name="upload" @change="uploadJSON">
  </label>
  <form @submit.prevent="exportJSON">
    <div class="p-4" v-for="page in settings.items" :key="page.uuid">
      <l-page 
        :page="page" 
        @sectionAdded="sectionAdded" 
        @questionAdded="questionAdded" 
      />
    </div>
    <div class="flex justify-end">
      <button class="bg-gray-300 rounded" @click.prevent="addPage()">Add Page</button>
    </div>
    <div class="flex justify-end">
      <button class="bg-gray-300 rounded">Export Form</button>
    </div>
  </form>
</div>
</template>

<script>
import { v4 as uuidv4 } from 'uuid'
import LPage from './LPage'
export default {
  name: 'LForm',
  components: {
    LPage,
  },
  data() {
    return {
      settings: {
        uuid: uuidv4(),
        type: 'form',
        items: [],
      },
      exportData: {
        uuid: uuidv4(),
        type: 'form',
        items: []
      },
    }
  },
  methods: {
    uploadJSON(e) {
      const reader = new FileReader();
      reader.readAsText(e.target.files[0]);
      reader.onload = this.storeData;
    },

    storeData(e) {
      this.settings = JSON.parse(e.target.result);
    },
    addPage() {
      let number = this.settings.items.length
      let data = {
        title: `${number + 1}. Page`,
        type: 'page',
        uuid: uuidv4()
      }
      this.settings.items.push(data)
      this.exportData.items.push(data)
    },

    questionAdded(data) {
      // update exportData
      console.log(data);
    },

    sectionAdded({pageUuid, data}) {
      // update exportData
      console.log(pageUuid, data);
    },

    exportJSON() {
      let dataStr = JSON.stringify(this.exportData);
      let dataUri = 'data:application/json;charset=utf-8,'+ encodeURIComponent(dataStr);

      let exportFileDefaultName = 'data.json';

      let linkElement = document.createElement('a');
      linkElement.setAttribute('href', dataUri);
      linkElement.setAttribute('download', exportFileDefaultName);
      linkElement.click();
    }
  },
  mounted() {
    this.$on('sectionAdded', ({pageUuid, data}) => {
      console.log(pageUuid, data)
    })
    this.$on('questionAdded', ({pageUuid, data}) => {
      console.log(pageUuid, data.sectioUuid, data.dada)
    })
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
