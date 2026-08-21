<script setup>
import { reactive } from 'vue'
import StudentInfo from '../components/StudentInfo.vue'
import SurveyHeader from '../components/SurveyHeader.vue'
import SurveyIntro from '../components/SurveyIntro.vue'
import SurveySection from '../components/SurveySection.vue'
import { surveyData } from '../data/surveyData.js'

const studentInfo = reactive(
  Object.fromEntries(surveyData.studentFields.map((field) => [field.id, ''])),
)
const answers = reactive({})

function setStudentInfo(value) {
  Object.assign(studentInfo, value)
}

function setAnswer(questionId, value) {
  answers[questionId] = value
}
</script>

<template>
  <main class="survey-page">
    <article class="survey-document">
      <SurveyHeader
        :document-label="surveyData.documentLabel"
        :title="surveyData.title"
        :subtitle="surveyData.subtitle"
      />

      <div class="survey-document__body">
        <StudentInfo
          :fields="surveyData.studentFields"
          :model-value="studentInfo"
          @update:model-value="setStudentInfo"
        />

        <SurveyIntro
          :title="surveyData.instructionsTitle"
          :introduction="surveyData.introduction"
          :instructions="surveyData.instructions"
        />

        <h2 class="document-heading document-heading--content">
          {{ surveyData.contentTitle }}
        </h2>

        <SurveySection
          v-for="section in surveyData.sections"
          :key="section.id"
          :section="section"
          :answers="answers"
          @answer="setAnswer"
        />
      </div>
    </article>
  </main>
</template>
