<template>
    <div class="border border-gray-500 rounded p-4 mb-4 bg-white">
      <h2 class="text-3xl" v-text="page.title"></h2>
      <l-section v-for="section in sections" @questionAdded="questionAdded" :section="section" :key="section.uuid"/>
        <div class="flex justify-end">
            <button class="bg-gray-300 rounded p-2" @click.prevent="addSection()">Add Section</button>
        </div>
    </div>
</template>
<script>
import { v4 as uuidv4 } from 'uuid'
import LSection from './LSection'
export default {
    components: {
        LSection
    },
    props: {
        page: {
            type: Object,
            required: true
        }
    },
    data() {
        return {
            sections: []
        }
    },
    methods: {
        addSection() {
            let number = this.sections.length
            let data = {
                title: `${number + 1}. Section`,
                type: "section",
                uuid: uuidv4()
            }
            this.sections.push(data)
            this.$emit('sectionAdded', {
                pageUuid: this.page.uuid,
                data
            })
        },
        questionAdded(data) {
            this.$emit('questionAdded', {
                pageUuid: this.page.uuid,
                data
            })
        }
    },
}
</script>