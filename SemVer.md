source:  
https://www.freecodecamp.org/learn/back-end-development-and-apis/managing-packages-with-npm/manage-npm-dependencies-by-understanding-semantic-versioning

---

## Introduction

Versions of the **npm** packages in the **dependencies** section of your `package.json` file follow what's called **Semantic Versioning** (SemVer), an industry standard for software versioning aiming to make it easier to manage dependencies.  

Libraries, frameworks or other tools published on npm should use SemVer in order to clearly communicate what kind of changes projects can expect if they update.  

Knowing SemVer can be useful when you develop software that uses external dependencies, which you almost always do.  
One day, your understanding of these **numbers** will save you from accidentally introducing breaking changes to your project without understanding why things that worked yesterday suddenly don't work today.  

This is how Semantic Versioning works according to the official website:  
`"package": "MAJOR.MINOR.PATCH"`  

- The MAJOR version number should be incremented when you make incompatible API changes
- The MINOR version number should be incremented when you add functionality in a backwards-compatible manner
- The PATCH version number should be incremented when you make backwards-compatible bug fixes

In other words:
- PATCHes are bug fixes
- MINORs add new features but neither of them break what worked before
- MAJORs add changes that won't work with earlier versions

---

## Use the Tilde to Always Use the Latest Patch Version of a Dependency

In your `package.json` file, section "dependencies": {...}, you can tell npm to only include a specific version of a package.  
It’s a useful way to freeze your dependencies if you need to ensure that different parts of your project stay compatible with each other.  

But in most use cases, you don’t want to miss bug fixes since they often include important security patches and (hopefully) don’t break  
things in doing so.  

To allow an npm dependency to update to **the latest PATCH version**, you can prefix the version number with the tilde `~` character.  
Here's an example of how to allow updates to any 1.3.x version:
```json
"dependencies": {
  "package": "~1.3.8"
},
```

---

## Use the Caret to Use the Latest Minor Version of a Dependency

Similar to how the tilde allows npm to install the latest PATCH, the caret `^` allows npm to install future updates as well.  
The difference is that the caret will allow both MINOR updates and PATCHes.

If you were to use the caret as a version prefix instead of the tilde, npm would be allowed to update to any 1.x.x version:
```json
"dependencies": {
  "package": "^1.3.8"
},
```


---
EOF
