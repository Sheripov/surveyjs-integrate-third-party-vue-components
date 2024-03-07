<script setup lang="ts">
import 'survey-core/defaultV2.min.css';
import { Model } from 'survey-core';

// const SURVEY_ID = 1;

const surveyJson = {
 "logoPosition": "right",
 "pages": [
  {
   "name": "page1",
   "elements": [
    {
     "type": "profile-data",
     "name": "Patient Information",
     "ShowFields": [
      "firstName",
      "lastName",
      "email",
      "phone"
     ]
    }
   ]
  }
 ]
};

const alertResults = (sender: any) => {
  const results = JSON.stringify(sender.data);
  alert(results);
  // saveSurveyResults(
  //   "https://your-web-service.com/" + SURVEY_ID,
  //   sender.data
  // )
}


const survey = new Model(surveyJson);
survey.setVariable('currentUser.firstName', 'Jane');
survey.setVariable('currentUser.lastName', 'Doe');
survey.setVariable('currentUser.email', 'jane.doe@example.com');
survey.setVariable('currentUser.phone', '+1234567890');
survey.onComplete.add(alertResults);

// function saveSurveyResults(url: string | URL, json: object) {
//   fetch(url, {
//     method: 'POST',
//     headers: {
//       'Content-Type': 'application/json;charset=UTF-8'
//     },
//     body: JSON.stringify(json)
//   })
//   .then(response => {
//     if (response.ok) {
//       // Handle success
//     } else {
//       // Handle error
//     }
//   })
//   .catch(error => {
//     // Handle error
//   });
// }
</script>

<template>
  <SurveyComponent :model="survey" />
</template>