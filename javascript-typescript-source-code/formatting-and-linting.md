
## **Formatting and Linting:**

### Linting: Lint, or a linter, is a static code analysis tool used to flag programming errors, bugs, stylistic errors and suspicious constructs.

>>> Source: https://en.wikipedia.org/wiki/Lint_(software)

We use one of the most populars libraries for javascript/typescript linting [eslint](https://eslint.org/), along with (prettier)[https://prettier.io] for code formatting.

We enforce the use of these tools by using:

- husky: https://github.com/typicode/husky 
- lint-staged: https://github.com/okonet/lint-staged 


> With husky and lint-staged we can run automatically these tools in code that is about to be committed. To set it up:

## 1) Install the following libraries:

```bash
npm i --save-dev eslint eslint-config-prettier husky lint-staged prettier
``` 

## 2) Setup `eslint` and `prettier` configuration files:

```bash
npx eslint --init
touch .eslintignore
echo {}> .prettierrc.json
touch .prettierignore
touch .gitignore
 
```

The rules on these files depend on whether you are coding on javascript or typescript, for Frontend applications or for Backend projects, and of course, on your preferences.

## 3) And Add the following sections to your `package.json`:

```json
  "lint-staged": {
    "*.{ts,tsx,js,jsx}": [
      "prettier --write",
      "eslint --fix",
      "git add"
    ]
  },
```

This indicates the files that will be linted and formatted when you do a commit.

## 3) Setup husky

```bash
npm pkg set scripts.prepare="husky install"
npm run prepare
npx husky add .husky/pre-commit "npx lint-staged"
git add .husky/pre-commit
```




## 4) Run `npm i` 
