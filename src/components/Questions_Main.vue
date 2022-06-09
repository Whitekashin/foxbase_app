<template>
  <Survey :survey="survey" />
</template>

<script>
// THEME
import "survey-core/modern.min.css";
import { Model, StylesManager } from "survey-core";
import { Survey } from "survey-vue-ui";

StylesManager.applyTheme("modern");

// JSON CONFIG - edit in order to change the questions. Refer to the Documentation for methods, parameters etc.
const surveyJson = {
  pages: [
    // Starting Page
    {
      elements: [
        {
          type: "html",
          html: "<h2>In this survey, we will ask you a couple questions about your impressions of our product.</h2>",
        },
      ],
    },

    {
      elements: [
        {
          name: "satisfaction-score",
          title: "How would you describe your experience with our product?",
          type: "radiogroup",
          choices: [
            { value: 5, text: "Fully satisfying" },
            { value: 4, text: "Generally satisfying" },
            { value: 3, text: "Neutral" },
            { value: 2, text: "Rather unsatisfying" },
            { value: 1, text: "Not satisfying at all" },
          ],
          isRequired: true,
        },
      ],
    },

    {
      elements: [
        {
          name: "what-would-make-you-more-satisfied",
          title: "What can we do to make your experience more satisfying?",
          type: "comment",
          visibleIf: "{satisfaction-score} = 4",
        },
        {
          name: "nps-score",
          title:
            "On a scale of zero to ten, how likely are you to recommend our product to a friend or colleague?",
          type: "rating",
          rateMin: 0,
          rateMax: 10,
        },
      ],
      visibleIf: "{satisfaction-score} >= 4",
    },

    {
      elements: [
        {
          name: "how-can-we-improve",
          title: "In your opinion, how could we improve our product?",
          type: "comment",
        },
      ],
      visibleIf: "{satisfaction-score} = 3",
    },
    {
      elements: [
        {
          name: "disappointing-experience",
          title:
            "Please let us know why you had such a disappointing experience with our product",
          type: "comment",
        },
      ],
      visibleIf: "{satisfaction-score} =< 2",
    },
  ],

  // Params for the Questionnaire
  showQuestionNumbers: "on",
  pageNextText: "Next",
  completeText: "Submit",
  firstPageIsStarted: true,
  showProgressBar: "bottom",
  startSurveyText: "Take the Survey",
  completedHtml: "Thank you for your feedback!",
  showPreviewBeforeComplete: "showAnsweredQuestions",
};

export default {
  name: "Questions_Main",
  components: {
    Survey,
  },
  data() {
    const survey = new Model(surveyJson);
    survey.onComplete.add(this.displayResults);
    return {
      survey,
      surveyResults: "",
      isSurveyCompleted: false,
    };
  },
  methods: {
    displayResults(sender) {
      this.surveyResults = JSON.stringify(sender.data, null, 4);
      this.isSurveyCompleted = true;
    },
  },
};

// BACKEND INTEGRATION
// saveSurveyResults(
//   "https://your-web-service.com/" + SURVEY_ID,
//   sender.data
// )

// function saveSurveyResults(url, json) {
//   const request = new XMLHttpRequest();
//   request.open('POST', url);
//   request.setRequestHeader('Content-Type', 'application/json;charset=UTF-8');
//   request.addEventListener('load', () => {
//     // Handle "load"
//   });
//   request.addEventListener('error', () => {
//     // Handle "error"
//   });
//   request.send(JSON.stringify(json));
// }
</script>
