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

