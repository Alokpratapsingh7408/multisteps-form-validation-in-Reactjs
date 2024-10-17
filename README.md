# multisteps-form-validation-in-Reactjs
Wanted to apply validation to each step so that if any input box is empty,the user cannot move forward.


// multisteps- validation

Scenario: Validating Multi-Step Form with Parent-Child Component Structure
You’re building a multi-step form in React where:

Parent Component handles navigation buttons (Next, Back).
Child Components handle input fields for each step of the form.
The challenge you're facing is applying validation in a way that prevents users from moving to the next step if any input in the current step is empty. Here's how you can manage it:

Approach:
Parent Component (MultiStepForm.js):

It holds the form state and manages which step the user is on.
It has navigation buttons (Next, Back).
It receives validation results from child components and decides whether the user can move to the next step.
Child Components (Step1.js, Step2.js, etc.):

Each child component contains the form inputs for that step.
Each child component handles its own validation logic and passes the result (valid/invalid) back to the parent.


To resolve the issue you're facing with form validation in a multi-step form where buttons are handled in the parent component and validation is done in the child component, apply the following approach:

Step-by-Step Solution:
Child Component:
The child component (e.g., PersonalInfo.js) will handle its own form validation. It uses setIsFormValid to inform the parent whether the form on the current step is valid.

Parent Component:
The parent component will control the navigation (Next, Back). Before moving to the next step, it checks the isFormValid state, which is updated by the child component based on whether all required fields are filled.

Child Component (PersonalInfo.js):
import React, { useEffect, useState } from 'react';

interface PersonalInfoProps {
  formData: any; // Update with appropriate type for formData
  onNext: (data: any) => void; // Define the data type accordingly
  setIsFormValid: (isValid: boolean) => void;
}

const PersonalInfo: React.FC<PersonalInfoProps> = ({ formData, onNext, setIsFormValid }) => {
  const [personalInfo, setPersonalInfo] = useState(formData);

  const validateForm = () => {
    // Check if all required fields are filled and valid
    const isValid = Object.values(personalInfo).every((value) => value && value.trim() !== '');
    setIsFormValid(isValid); // Notify parent about the form's validity
    console.log(isValid);
  };

  useEffect(() => {
    validateForm(); // Validate the form whenever personalInfo state changes
  }, [personalInfo]);

  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    setPersonalInfo({
      ...personalInfo,
      [e.target.name]: e.target.value,
    });
  };

  return (
    <div>
      <h2>Personal Information</h2>
      <input
        type="text"
        name="firstName"
        value={personalInfo.firstName || ''}
        onChange={handleChange}
        placeholder="First Name"
      />
      <input
        type="text"
        name="lastName"
        value={personalInfo.lastName || ''}
        onChange={handleChange}
        placeholder="Last Name"
      />
      {/* Add other form fields as needed */}
    </div>
  );
};

export default PersonalInfo;

Parent Component (StepForm.js):

import React, { useState } from 'react';
import PersonalInfo from './PersonalInfo'; // Import child component

const StepForm: React.FC = () => {
  const [step, setStep] = useState(1);
  const [isFormValid, setIsFormValid] = useState(false);
  const [formData, setFormData] = useState({
    personal: { firstName: '', lastName: '' }, // Define other form fields as necessary
  });

  const handleNext = (data: any, section: string) => {
    // Update the form data for the current section
    setFormData({ ...formData, [section]: data });
  };

  const nextStep = () => {
    if (isFormValid) {
      setStep(step + 1);
    } else {
      alert('Please fill out all required fields before proceeding.');
    }
  };

  const renderStep = () => {
    switch (step) {
      case 1:
        return <PersonalInfo formData={formData.personal} setIsFormValid={setIsFormValid} onNext={(data) => handleNext(data, 'personal')} />;
      // Add other steps as needed
      default:
        return null;
    }
  };

  return (
    <div>
      {renderStep()}
      <button onClick={nextStep}>Next</button>
    </div>
  );
};

export default StepForm;
Key Points:
Child Component Validation: In the child component (PersonalInfo.js), the form is validated using the validateForm function, which checks if all fields are filled. It passes the validation result (isFormValid) back to the parent via the setIsFormValid callback.

Parent Component Control: The parent component (StepForm.js) controls the navigation (e.g., the "Next" button). It checks isFormValid before allowing the user to move to the next step.

/******************************************************************************************************************************************************\

How This Solves the Problem:
Validation in Child, Control in Parent: The child component manages validation, while the parent component controls the flow and prevents the user from moving forward if the current step's form is invalid.

Smooth User Experience: This structure keeps the validation logic simple and ensures that all required fields are filled out before the user can proceed to the next step, leading to a smoother user experience.


