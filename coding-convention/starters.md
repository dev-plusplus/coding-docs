# Prefer the use of Starter repos for new projects


# Introduction

The starter focus in including a series of technologies that are fundamental for Cobuild Lab's development flow:

## The current staters for the Cobuild Lab are: 

- Typescript NextJS: https://github.com/cobuildlab/create-react-nextjs-typescript-app
- Typescript React Native: https://github.com/cobuildlab/create-react-native-typescript-app
- Typescript CRA: https://github.com/cobuildlab/create-react-cra-typescript-app
- Typescript 8base functions: https://github.com/cobuildlab/create-8base-typescript-app
- Typescript Nodejs package: https://github.com/cobuildlab/create-nodejs-typescript-package

[See full list](https://github.com/cobuildlab?q=create&type=all&language=&sort=)

## Elements of every starter:

- Github Workflows: ESLITN, TEST, BUILD and PUBLISH (for npm packages)
- Husky + Lint Staged
- Prettier setup
- ESLINT setup
- `tsconfig.json`
- Example of a module and page (for NextJS)
- Jest: setup and example test: Only actions, services and hooks
- Auth0 integration

## Maintenance Flow:

When someone is gonna start a new project:

1) Create a fork of the required starter
2) Check for new librarie's versions and conventions
3) Update the starter
4) Create a Pull Request for updating the starter
5) Continue with the normal development project

## Things TODO: https://github.com/cobuildlab/coding-docs/issues/75

- [ ] Add to the React Native Starter, the token validation
- [ ] Add other relevant libraries: Yup, react Hook Form, apollo graphql generator, dayjs
- [ ] Upgrade Material UI

