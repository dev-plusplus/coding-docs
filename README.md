# [Cobuild](https://cobuildlab.com) Lab Coding Conventions

## 0. General Product conventions:

### **Feature Flags** for isolating features in the system

With the purpose on enabling / disabling features on the system easily, we implement a Database table called `FeatureFlag` with the names of the Features nad whether they are disabled or not.

Example 1: We create a new Salesforce integration that pass the client approval and is in production. If the integration is found to be problematic in production we can turn off the Feature in production so that the user stop experimenting problems.

Example 2: We can use a Feature Flag to stop sending specific emails, like newsletters, or weekley digests in testing or staging environments.

**Table Structure**
Name: FeatureFlag
Columns: 1) name: String with a defined set of options; 2) active: Boolean to define if is on or off

**Considerations**
- We need a web service to communicate the Front end which Features are active or noot
- We need a function (action)  to resolve if a FeatureFlag is ON/OFF in the backend. Example: `isFeatureFlagActive(name:string)`
-

## [1. General coding conventions:](./conventions/general-coding-conventions.md)

Conventions not directly related to how the code is written.

## [2. Effective Programming: A guide for conventions and best practices in software development:](./conventions/effective-programming-at-cobuildlab.md)

General conventions to write "effective code".

## [3. Conventions for creating a React or React Native Application:](./conventions/conventions-for-creating-a-react-application.md)

Conventions specific to React code bases.

## [4. Conventions for Javascript / Typescript source code:](./conventions/conventions-for-javascript-typescript-source-code.md)

Conventions specific to Javascript / Typescript code bases.

## [5. 8base conventions](https://github.com/cobuildlab/8base-recipes)

Conventions related to 8base applications

TODO: 

## 5. Git branches strategy for environments deployment on web applications: (TODO)

- YUP + hook react form
- ROLE BASED COMPONENTS: donde decidir que cargar (Donde poner el IF)
- Custom Hooks vs React simple state: Events vs Providers
- 8base: NO skip permission un less that you are on resolvers
