# [Cobuild](https://cobuildlab.com) Lab Coding Conventions

### Preferred Tooling:

1. Languages: Typescript and Python
2. Frontend Frameworks: ReactJs and React Native
3. Backend Frameworks: 8base, Supabase, Hasura and AWS
4. Devops: GitHub, Buddy Works, and Slack
5. Code Quality: ESLINT and Husky

## 0. General Product conventions:

### **Feature Flags** for isolating features in the system

With the purpose of enabling / disabling features on the system easily, we implement a Database table called `FeatureFlag` with the names of the Features and whether they are disabled or not.

Example 1: We create a new Salesforce integration that passes the client's approval and is in production. If the integration is found to be problematic in production we can turn off the Feature in production so that the user stops experimenting problems.

Example 2: We can use a Feature Flag to stop sending specific emails, like newsletters, or weekly digests in testing or staging environments.

**Table Structure**
Name: FeatureFlag
Columns: 1) name: String with a defined set of options; 2) active: Boolean to define if it is on or off

**Considerations**
- We need a web service to communicate to the Front end which Features are active or not
- We need a function (action)  to resolve if a FeatureFlag is ON/OFF in the backend. Example: `isFeatureFlagActive(name:string)`
-

## [1. General coding conventions:](./conventions/general-coding-conventions.md)

Conventions are not directly related to how the code is written.

## [2. Effective Programming: A guide for conventions and best practices in software development:](./conventions/effective-programming-at-cobuildlab.md)

General conventions to write "effective code".

Conventions specific to Javascript / Typescript code bases.

## 4. And dozens of pending things... [HERE](https://github.com/dev-plusplus/coding-docs/issues)
