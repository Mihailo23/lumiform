<template>
    <div class="border-2 rounded p-4 mb-4">
        <h2 class="text-3xl" v-text="section.title"></h2>
        <l-question v-for="question in questions" :question="question" :key="question.uuid"/>
        <div class="flex justify-end">
            <div>
                <form @submit.prevent="addQuestion" v-if="questionFormVisible">
                    <input class="border rounded p-2 mr-2" type="text" placeholder="Your question?" v-model="questionForm.title">
                    <select class="border rounded p-2 mr-2" name="type" id="type" v-model="questionForm.response_type">
                        <option value="number" selected>number</option>
                        <option value="text">text</option>
                    </select>
                    <button class="bg-gray-300 rounded p-2 mr-2">save question</button>
                </form>
            </div>
            <button class="bg-gray-300 rounded p-2" @click.prevent="toggleQuestionForm()" v-text="questionFormVisible ? 'Cancel' : 'Add Question'" />
        </div>
        <l-section v-for="subSection in subSections" :section="subSection" :key="subSection.uuid"/>
        <div class="flex justify-end mt-2">
            <button class="bg-gray-300 rounded p-2 ml-2" @click.prevent="addSubSection()">Add Subsection</button>
        </div>
    </div>
</template>
<script>
import { v4 as uuidv4 } from 'uuid'
import LQuestion from './LQuestion'
import LSection from './LSection'
export default {
    name: 'LSection',
    components: {
        LQuestion,
        LSection
    },
    props: {
        section: {
            type: Object,
            default: () => {}
        }
    },
    data() {
        return {
            questions: [],
            questionFormVisible: false,
            questionForm: {
                title: '',
                response_type: 'number',
                type: 'question',
                uuid: uuidv4()
            },
            subSections: [],
        }
    },
    methods: {
        addQuestion() {
            this.questions.push(this.questionForm)
            this.$emit('questionAdded', {
                sectionUuid: this.section.uuid,
                question: this.questionForm
            })
            this.resetQuestionForm()
        },
        addSubSection() {
            let number = this.subSections.length
            this.subSections.push({
                title: `${number + 1}. SubSection`,
                uuid: uuidv4()
            })
        },
        toggleQuestionForm() {
            this.questionFormVisible = !this.questionFormVisible
        },
        resetQuestionForm() {
            this.questionForm = {
                title: '',
                response_type: 'number',
                type: 'question',
                uuid: uuidv4()
            }
        }
    }
}
</script>