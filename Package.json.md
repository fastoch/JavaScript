src = https://www.freecodecamp.org/learn/back-end-development-and-apis/

# Intro

- JavaScript can also be used on the back-end, or server-side, to build entire Web applications.
- **npm** (Node Package Manager) is a CLI tool to install, create and share packages of JavaScript code written for **Node.js**
- There are many open-source packages available on npm, so before starting a project, take some time to explore so you don't end up reinventing the wheel for things like working with dates or fetching data from an API

# How to use `package.json`?

- `package.json` is the core of any Node.js project or npm package, it stores information about your project
- it consists of a single JSON object where information is stored in key-value pairs
  - There are only 2 required fields: `name` and `version`, but it's good practice to provide additional information
- you can create the `package.json` file from the terminal using the `npm init` command, this will run a guided setup
  - using `npm init` with the `-y` flag will generate the file without having it ask any questions: `npm init -y`
- if you look at the file tree of your project, you will find the `package.json` file on the top level of the tree
- One of the most common pieces of information in this file is the `author` field.
  - It specifies who created the project, and can consist of a string or an object with contact or other details
  - an object is recommended for bigger projects, but a simple string like the following example will do for a small project:
  - `"author": "Jane Doe",`

>[!important]
>Remember that you're writing JSON, so all field names must use double quotes and be separated with a comma.
