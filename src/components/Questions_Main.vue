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
          html: "<h2>In this survey, we will ask you a couple questions about your impressions of JavaScript.</h2>",
        },
      ],
    },

    {
      elements: [
        {
          name: "satisfaction-score",
          title: "How would you describe your experience with JavaScript?",
          type: "radiogroup",
          choices: [
            { value: 5, text: "Awesome!" },
            { value: 4, text: "I like it" },
            { value: 3, text: "It's okay" },
            { value: 2, text: "I don't like it" },
            {
              value: 1,
              text: "It's the worst programming language to ever be conceived",
            },
          ],
          isRequired: true,
        },
      ],
    },

    {
      elements: [
        {
          name: "what-would-make-you-more-satisfied",
          title:
            "Which feature would like to be added to the language in the future?",
          type: "comment",
          visibleIf: "{satisfaction-score} = 4",
        },
        {
          name: "nps-score",
          title:
            "On a scale of zero to ten, how likely are you to recommend programming in JavaScript?",
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
          title: "In your opinion, how should JavaScript be improved?",
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
            "Please let us know why you didn't like JavaScript and how it should be improved?",
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
</script>
