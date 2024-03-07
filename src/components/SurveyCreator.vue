<script setup lang="ts">
import 'survey-core/defaultV2.min.css';
import "survey-creator-core/survey-creator-core.min.css";

import {Serializer, ComponentCollection} from "survey-core";
import {SurveyCreatorModel} from "survey-creator-core";
import type {SurveyModel} from "survey-core";
import type {CreatorBase} from "survey-creator-core";

Serializer.addProperty("survey", {
  name: "backgroundColor",
  displayName: "Background color",
  type: "color",
  category: "general",
  visibleIndex: 3,
  onSetValue: (survey: SurveyModel, value: string) => {
    survey.setPropertyValue("backgroundColor", value);
    applyBackground(value);
  }
});

ComponentCollection.Instance.add({
  name: "profile-data",
  title: "Profile Data",
  elementsJSON: [
    {
      isRequired: false,
      visible: false,
      type: "text",
      name: "firstName",
      title: "First Name",
      minWidth: 200,
      defaultValueExpression: "{currentUser.firstName}"
    },
    {
      isRequired: false,
      visible: false,
      type: "text",
      name: "lastName",
      title: "Last Name",
      minWidth: 200,
      defaultValueExpression: "{currentUser.lastName}"
    },
    {
      isRequired: false,
      visible: false,
      type: "text",
      name: "email",
      title: "Email",
      minWidth: 200,
      inputType: "email",
      defaultValueExpression: "{currentUser.email}"
    },
    {
      isRequired: false,
      visible: false,
      type: "text",
      name: "phone",
      title: "Phone",
      minWidth: 200,
      inputType: "tel",
      defaultValueExpression: "{currentUser.phone}"
    }
  ],
  //SurveyJS calls this function one time on registing component, after creating "fullname" class.
  onInit() {
    //SurveyJS will create a new class "fullname". We can add properties for this class onInit()
    Serializer.addProperty("profile-data", {
      name: "RequiredFields:multiplevalues",
      choices: [
        {value: "firstName", text: "First Name"},
        {value: "lastName", text: "Last Name"},
        {value: "email", text: "Email"},
        {value: "phone", text: "Phone"}
      ],
      default: ["firstName"],
      category: "general",
    });
    Serializer.addProperty("profile-data", {
      name: "ShowFields:multiplevalues",
      choices: [
        {value: "firstName", text: "First Name"},
        {value: "lastName", text: "Last Name"},
        {value: "email", text: "Email"},
        {value: "phone", text: "Phone"}
      ],
      default: ["firstName"],
      category: "general",
    });
  },
  onLoaded(question) {
    updateFieldsVisibility(question);
    updateRequiredFields(question);
  },
  onPropertyChanged(question, propertyName, newValue) {
    if (propertyName == "ShowFields") {
      updateFieldsVisibility(question);
    }
    if (propertyName == "RequiredFields") {
      updateRequiredFields(question);
    }
  },
});

function updateFieldsVisibility(question: any) {
  const fields = ["firstName", "lastName", "email", "phone"];
  fields.forEach(fieldName => {
    let field = question.getQuestionByName(fieldName);
    if (field) {
      field.visible = question.ShowFields.includes(fieldName);
    }
  });
}
function updateRequiredFields(question: any) {
  const fields = ["firstName", "lastName", "email", "phone"];
  fields.forEach(fieldName => {
    let field = question.getQuestionByName(fieldName);
    if (field) {
      field.isRequired = question.RequiredFields.includes(fieldName);
    }
  });
}
function applyBackground(color: string) {
  setTimeout(() => {
    const surveyEl = document.getElementsByClassName("sd-root-modern")[0] as HTMLElement;
    if (surveyEl) {
      surveyEl.style.setProperty("--background", color);
    }
  }, 50);
}

function handleActiveTabChange(sender: CreatorBase, {tabName}: { tabName: string }) {
  if (tabName === "test" || tabName === "designer") {
    applyBackground(sender.survey.backgroundColor);
  }
}

const creator = new SurveyCreatorModel({});
creator.onActiveTabChanged.add(handleActiveTabChange);
</script>

<template>
  <SurveyCreatorComponent :model="creator"/>
</template>