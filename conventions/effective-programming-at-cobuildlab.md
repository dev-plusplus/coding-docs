# 2. Code Review:

Code Review should be a practice used by all development teams as is probed technique that, done right improves the **effectiveness** of any code base.

In Cobuild Lab, we use this technique as a mandatory step before merging any feature into a collaboration branch. 

Code reviews in Cobuild Lab is being based mainly in the process used by Google Engineers and posted on [Github](https://google.github.com) and the [Git Feature Branch Flow](https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow) proposed by [Atlassian]:

- [Git Feature Branch Flow](https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow)
- [The Standard of Code Review](https://google.github.io/eng-practices/review/reviewer/standard.html)
 
Here, we present a brief summary of the aspects of a good process of Code Review:

## Principles

- The main goal of the Code Reviewing process is to increase the quality of the code base of any given project.
- The Code Review can be used as a Learning Process as is a good tool to communicate the best practices and solutions to code owners.  
- The reviewer has ownership over the code being reviewed thus is responsible for its quality as well. 

- Comments on review should carry a proper explanation and alternative solution (if any)
- Any conflicts that can't be agreed between author and reviewer needs to be escalated. 

## Secondary goals of the Code Review process are:

- Keep consistency in the code across the organization
- Detect defects before any merge event
- Identify common patterns that can be added to the Knowledge base of the organization (Conventions documents)
- Prevent unnecessary additions to the projects code base.

## Preparing your code for reviewing:

Before submitting your code for reviewing make sure to read and follow these conventions:

- [Feature Branch Strategy](https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow)
- [Git Branches strategy for environments on web applications](https://www.devsup.io/git-branches-strategy-for-environment-deployment-on-web-applications/)
- [Effective Programming](https://www.devsup.io/effective-programming-at-cobuildlab/) (This guide)
- [Conventions to create a React or React Native Application](https://www.devsup.io/conventions-to-create-a-react-application/)
- [Conventions for Javascript / Typescript source code](https://www.devsup.io/conventions-for-javascript-typescript-source-code/)

In addition to this, make sure you provide the proper information and compliant with the following requirements:

- Describe the work done
- Issues that solves when merged (e.x: solves #456)
- User Story compliant: Make sure that your Code solves the requirements of the Use Case, ticket or issue.
- Make sure your coode follows the style guidelines of the organization
- You have performed a self-review of your code
- The code is properly documented as the specification in the conventions documents.
- You have made corresponding changes to the documentation of the project
- Your changes do not introduce any new warnings
- You have added the proper tests to the features included
- New and existing unit tests pass locally with the updates
- Provide instructions to Validate the Features has been, screenshots if necessary
- Indicate any new constraint or side effect


## General Rules

- Follow organization conventions
- > Technical facts and data overrule opinions and personal preferences.
  > Aspects of software design are almost never a pure style issue or just a personal preference. They are based on underlying principles and should be weighed on those principles, not simply by personal opinion. Sometimes there are a few valid options. If the author can demonstrate (either through data or based on solid engineering principles) that several approaches are equally valid, then the reviewer should accept the preference of the author. Otherwise the choice is dictated by standard principles of software design.
  > If no other rule applies, then the reviewer may ask the author to be consistent with what is in the current codebase, as long as that doesn’t worsen the overall code health of the system.
- Make sure review every single line of code

## What to look in a Code Review?

- Does the features and changes make sense?
- The use of common agreed patterns
- The use of Anti Patterns or bad practices
- Unnecessary complexity
- Opportunities to contribute to the libraries maintained by the organization or to create new ones:
    * [React Typescript App Starter](https://github.com/cobuildlab/create-react-typescript-app)
    * [React Native Typescript App Starter](https://github.com/cobuildlab/create-react-native-typescript-app)
    * [React NodeJS Typescript App Starter](https://github.com/cobuildlab/create-nodejs-app)
    * [React State Library](https://github.com/cobuildlab/react-simple-state)
    * [Validation Utils](https://github.com/cobuildlab/validation-utils)
    * [Hooks utils](https://github.com/cobuildlab/hooks-utils)
    * [Stripe Utils](https://github.com/cobuildlab/stripe-utils)
    * [8base-utils](https://github.com/cobuildlab/8base-utils)
 - Avoid code duplicity
 - Test are reasonably extensive
    
