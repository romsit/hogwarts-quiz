Sorting Hat Quiz

Welcome to the Sorting Hat Quiz — an interactive, chat‑based quiz inspired by the magical Sorting Hat from the Harry Potter universe. Built using Vue 3
 with the Composition API, this project guides users through a series of questions in a friendly chat interface and assigns them to one of four houses based on their responses.

Features

🔮 Interactive chat UI – users answer questions through a conversational interface.

🧙 Personalized experience – the quiz asks for the user's name and greets them throughout the process.

🎨 Smooth animations – questions and answers fade in/out for a polished feel.

🏰 House allocation – scores are tallied for Gryffindor, Ravenclaw, Hufflepuff and Slytherin to determine your house.

🔧 Easy customization – questions and scoring can be modified via a simple JavaScript file.

Getting Started

These instructions will help you get a copy of the project up and running on your local machine for development and testing purposes.

Prerequisites

Node.js
 (version 16 or higher is recommended).

npm (included with Node.js) or Yarn
.

Installation

Clone the repository:

git clone https://github.com/romsit/hogwarts-quiz
cd hogwarts-quiz


Install dependencies:

npm install
# or
yarn install

Running the development server

Start the development server with hot module reloading:

npm run dev
# or
yarn dev


The application will compile and print the local URL in the terminal (by default http://localhost:5173). Open it in your browser to view the quiz. The page will reload automatically when you edit files.

Building for production

To create an optimized build for production:

npm run build
# or
yarn build


The compiled assets will be placed in the dist directory. You can serve these static files using any HTTP server, such as node_modules/.bin/http-server dist or by deploying the folder to your hosting provider.

Linting and formatting

This project uses ESLint
 and Prettier
 to maintain code quality and consistency. Before committing code, please run:

npm run lint
# automatically fix lint issues and format code
npm run lint -- --fix
npm run format


Running these commands will check for syntax errors, enforce coding conventions and apply consistent formatting. Ensuring your code passes the linter helps keep the repository maintainable.

Tests

At present, there are no automated tests. Contributions that add unit or integration tests are very welcome. We recommend using Jest
 together with Vue Test Utils
 for testing components. If you introduce tests, please document how to run them in this section.

Project structure

The high‑level directory layout is as follows:

.
├── public/               # Static public assets (favicon, index.html)
├── src/
│   ├── assets/           # Images, icons and global styles
│   ├── components/       # Vue components (e.g. chat interface)
│   ├── data/             # Static data (e.g. questions and scoring)
│   ├── App.vue           # Root component
│   └── main.js           # Application entry point
├── package.json
├── vite.config.js        # Build and dev server configuration (if using Vite)
└── ...


Your actual project structure may differ slightly depending on your build setup. The key file to customize the quiz logic is src/data/quiz.js.

Customizing the quiz

All questions and scoring logic live in src/data/quiz.js. To tailor the quiz to your needs, edit this file:

Add or remove questions by modifying the exported array.

For each answer, update the scores object to adjust how much each house score is incremented.

Ensure every house (gryffindor, ravenclaw, hufflepuff, slytherin) appears in the scores object so the result computation works correctly.

Feel free to rename houses or add new categories, but remember to update the scoring logic and the result display accordingly.

Contributing

Contributions are what make the open‑source community such an amazing place to learn, inspire and create. Any contributions you make to this project are greatly appreciated.

How to contribute

Fork the repository and create your branch from main:

git checkout -b feature/my-feature


Make your changes. Write clear, concise commit messages in English.

Test your changes locally. Ensure the development server runs without errors and that your code passes npm run lint.

Push to your branch and open a pull request:

git push origin feature/my-feature


Fill in the pull request template (if available), explaining what problem your change solves or what feature it adds. Include relevant screenshots or links.

Guidelines

Adhere to the existing code style enforced by ESLint and Prettier.

Reference open issues in your commits and pull requests when appropriate (e.g. Fixes #42).

Include tests or examples when adding new features.

Keep pull requests focused. If you’re fixing two unrelated issues, submit two separate PRs.

Be respectful and constructive in discussions. We value a friendly and inclusive environment.

If you are unsure about anything, feel free to open an issue to ask questions before starting work.

Code of Conduct

This project adopts the Contributor Covenant
 Code of Conduct. We expect everyone participating in the project to abide by it. Please report unacceptable behavior to the maintainer’s email listed below.

License

This project is licensed under the MIT License
. You are free to use, modify and distribute it under the terms of that license.

Contact

Maintained by Romina – romihello@outlook.com
. Feel free to contact us for support, questions or suggestions.