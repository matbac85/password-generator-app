## Password Generator App

![Password Generator App](preview.jpg)

### Application Description

This application generates secure passwords based on user-defined criteria such as length and character types. It includes features for dynamically displaying password strength and copying generated passwords to the clipboard.

#### Try It Out

Explore the app live [here](https://matbac85.github.io/password-generator-app/).

#### Features and Input Options

1. **Password Length Slider**:

   - Users adjust the length of the password using a slider ranging from 1 to 20 characters.

2. **Character Type Selection**:

   - Checkboxes allow users to include uppercase letters, lowercase letters, numbers, and symbols in their generated passwords.

3. **Password Strength Indicator**:

   - Real-time feedback on password strength based on selected criteria (length and character types).

4. **Copy to Clipboard**:
   - Users can copy generated passwords with a single click for easy use in other applications.

#### Tools Used

- **Vue.js**: JavaScript framework used for building the user interface and managing data reactivity.
- **Computed Properties and Watchers**: Computed properties are utilized to dynamically calculate password strength and slider values based on user input. Watchers are employed to monitor changes in password generation, ensuring real-time updates in the UI.
- **HTML/CSS**: Used for structuring and styling the user interface in a responsive and aesthetic manner.

#### Functionality and Reset Option

- **Generate Button**: Clicking generates a new password based on the selected criteria.
- **Copy Button**: Copies the generated password to the clipboard, with visual confirmation upon successful copy.

#### Credits

This project was completed as a front-end coding challenge from [Frontend Mentor](https://www.frontendmentor.io/).

### Summary

The Password Generator App simplifies the process of creating strong, customized passwords with options for length and character types. It provides instant feedback on password strength and facilitates easy copying for enhanced security and convenience.
