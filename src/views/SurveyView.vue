<script setup>
import { computed, reactive, ref } from 'vue'
import StudentInfo from '../components/StudentInfo.vue'
import SurveyHeader from '../components/SurveyHeader.vue'
import SurveyIntro from '../components/SurveyIntro.vue'
import SurveySection from '../components/SurveySection.vue'
import { surveyData } from '../data/surveyData.js'

const studentInfo = reactive(
  Object.fromEntries(surveyData.studentFields.map((field) => [field.id, ''])),
)
const answers = reactive({})
const submissionMessage = ref('')
const submissionStatus = ref('')
const totalQuestions = surveyData.sections.reduce(
  (total, section) => total + section.questions.length,
  0,
)
const answeredCount = computed(() => Object.keys(answers).length)

function setStudentInfo(value) {
  Object.assign(studentInfo, value)
}

function setAnswer(questionId, value) {
  answers[questionId] = value
  submissionMessage.value = ''
  submissionStatus.value = ''
}

function submitSurvey() {
  if (answeredCount.value < totalQuestions) {
    submissionStatus.value = 'warning'
    submissionMessage.value = `Vui lòng trả lời đủ ${totalQuestions} câu trước khi gửi. Hiện bạn đã trả lời ${answeredCount.value}/${totalQuestions} câu.`
    return
  }

  submissionStatus.value = 'success'
  submissionMessage.value =
    'Đã kiểm tra đầy đủ câu trả lời. Đây là bản demo nên kết quả chưa được lưu lên máy chủ.'
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

      <form class="survey-document__body" @submit.prevent="submitSurvey">
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

        <section class="survey-submit" aria-labelledby="survey-submit-title">
          <div>
            <h2 id="survey-submit-title" class="survey-submit__title">Hoàn thành khảo sát</h2>
            <p class="survey-submit__progress">
              Đã trả lời <strong>{{ answeredCount }}/{{ totalQuestions }}</strong> câu
            </p>
          </div>

          <button class="survey-submit__button" type="submit">
            <i class="bi bi-send-fill" aria-hidden="true"></i>
            Gửi kết quả
          </button>

          <p
            v-if="submissionMessage"
            class="survey-submit__message"
            :class="`survey-submit__message--${submissionStatus}`"
            role="status"
            aria-live="polite"
          >
            {{ submissionMessage }}
          </p>
        </section>
      </form>
    </article>
  </main>
</template>
