
# Mood Tracker

Build a personalized mood tracker app that lets users log in, select their mood(s) for the day, and see their overall mood over time. Users can select multiple moods provided by the API, and each mood selection will be represented as an emoji-based entry. These entries are displayed as the user’s overall mood in their profile.

## 1) Set up the development environment

Make sure you have installed at least the following tools:
- A text editor of your choice (Visual Studio Code recommended)
- NPM/Node.js (LTS – Long Term Support version)
- React (using Vite)
- Browser Developer Tools for testing and debugging
- Git

## 2) Component Tree Design

Create a component tree diagram for your application using Figma or a similar tool. The diagram should show the main components, including the mood selection form, affirmation display, and profile history.

## 3) HTML & CSS Styling

Use CSS and HTML to structure and style the app. You may use a library like TailwindCSS or Bootstrap for responsive design if desired.

## 4) Build the following screens in React (See Appendix A for detailed specs):

a) The Startup Page ("Login")

b) The Mood Entry/Selector Page

c) The Profile Page

## 5) Submission

a) Upload your component tree PDF to the project’s Git repository and submit the link.

b) Store your project in a private Git repository and add your lecturer as a Maintainer.

c) Try hosting your app using a service like Vercel (https://vercel.com/guides/deploying-react-with-vercel).

---

## Appendix A: Requirements for Mood Tracker Website

### 1. Startup/Login Page

<img src="../../../../assets/images/module-2-assignment-wireframe-login.png" alt="drawing" width="700"/>

The first thing a user should see is the “Login page” where the user must be able to enter their name. 
Save the username to the MoodTracker API. Remember to first check if the user exists. Once the name has been stored in the API, the app must display the main page, the Mood Selector/Entry page. 
Users that are already logged in may automatically be redirected to the Mood Selector/Entry page. You may use the browsers’ local storage to manage the session. 


### 2. Mood Selector/Entry page

<img src="../../../../assets/images/module-2-assignment-wireframe-selectmood.png" alt="drawing" width="700"/>

#### 2.1 API-Provided Mood Options

Fetch mood options from the API that returns a list of available moods and associated emojis. Example response:

```json
[
  { "id": 1,  "name": "Happy",      "emoji": "😊", "category": "Positive" },
  { "id": 2,  "name": "Calm",       "emoji": "🌊", "category": "Positive" },
  { "id": 3,  "name": "Energized",  "emoji": "⚡", "category": "Positive" },
  { "id": 4,  "name": "Reflective", "emoji": "🧘", "category": "Neutral"  },
  { "id": 5,  "name": "Hopeful",    "emoji": "🌄", "category": "Positive" },
  { "id": 6,  "name": "Tired",      "emoji": "😴", "category": "Negative" },
  { "id": 7,  "name": "Anxious",    "emoji": "😟", "category": "Negative" },
  { "id": 8,  "name": "Content",    "emoji": "😌", "category": "Neutral"  },
  { "id": 9,  "name": "Stressed",   "emoji": "😣", "category": "Negative" },
  { "id": 10, "name": "Curious",    "emoji": "🤔", "category": "Neutral"  },
  { "id": 11, "name": "Excited",    "emoji": "🎉", "category": "Positive" },
  { "id": 12, "name": "Lonely",     "emoji": "😔", "category": "Negative" }
]
```

The response is a plain array, not an object wrapping one. Render whatever the API returns rather than hard-coding this list.

#### 2.2 Multiple Mood Selection

Users can select multiple moods to capture complex feelings. Combine selected emojis into a single string to represent the user's last entry.

---

### 3. Profile Page

<img src="../../../../assets/images/module-2-assignment-wireframe-profile.png" alt="drawing" width="700"/>

#### 3.1 Mood History

Display the last 10 mood entries (as emojis) for the user.

#### 3.2 Clear History Option

Include a “Clear History” button to delete all saved entries for the user.

#### 3.3 Affirmation Text and Emoji-Based Entry (Optional)

Generate a unique affirmation (quote or advice) based on the category of the most selected moods.

#### 3.4 Aggregated Mood Image (Optional)

Display a background image or animated illustration that reflects the category of the most selected moods.

---
