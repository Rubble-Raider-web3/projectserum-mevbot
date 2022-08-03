# Introduction
This repository contains all the guides that we want to  write for AAVE. 

We have two ways of writing the guides
1. Through the UI
2. Through Git

This repository follows the second option for writing guides i.e. in git repository

Git options provides a couple of benefits
1. Review's can be done via PRs
2. People across the project can for the repository and contribute
3. Content is backed-up
4. Content is Open Source and can be used by everyone

A Guide consists of following things
1) Basic Info - This includes name, excerpt, thumbnail,  etc.
2) Steps - Here is the example of the step
   ```yaml
     - uuid: bd8f0b48-4e61-478a-b275-68e3c93db760
       content: |
       Currently contributing to courses involves working on a chapter which
       has \"two\" tasks
         1. Readings and Summaries - Create yaml file with details of readings and summaries for that chapter. e.g. [https://github.com/DoDAO-io/dodao-solidity-course/blob/main/src/summaries/data_types.yaml](https://github.com/DoDAO-io/dodao-solidity-course/blob/main/src/summaries/data_types.yaml)
         2. Questions - Create 50 high quality single or multiple choice questions for the  chapter you are working on. e.g. [https://github.com/DoDAO-io/dodao-solidity-course/blob/main/src/questions/data_types.yaml](https://github.com/DoDAO-io/dodao-solidity-course/blob/main/src/questions/data_types.yaml). Later
            on we will add another task i.e. creating of the mind maps for each chapter, but
            for now its not needed. Also make sure that your name is added on a Trello card corresponding to the task you are working on.
            stepItems: []
            name: Contributing
   ```
   Steps consist of following information
   1) UUID - unique identifier. You can use this site to generate a V4 uuid https://www.uuidgenerator.net/version4
   2) Name
   3) Content - Details that are rendered below the headings. This is a string formatted with markdown
   4) StepItems - Step items can be of the following types
      1) Question - Type of question can be `MultipleChoice` or `SingleChoice`
            ```yaml
               - uuid: f12c2758-858d-4fe4-9560-f4dd9454f89a
                 content: What two tasks are currently part of the work involved for a chapter
                 of a course?
                 type: MultipleChoice
                 answerKeys:
                 - B
                 - C
                 choices:
                 - content: Create a youtube video
                   key: A
                 - content: Create yaml file with readings and summaries
                   key: B
                 - content: Create yaml file for questions
                   key: C
                 - content: Create a mind map
                   key: D
            ```
      2) UserInput - This is used to capture information from user. Type can be `PublicShortInput` or `PrivateShortInput` e.g.
            ```yaml
                    - uuid: 5e025ae8-daf2-4074-8e6c-68de05d3e5cc
                      label: Name
                      required: true
                      type: PublicShortInput
            ```
      3) Discord - This step item is used to ask user to connect their discord. e.g.
        ```yaml
           - uuid: 0d3bf837-e661-43b3-ba85-6d2ebb272be5
             type: UserDiscordConnect
        ```
      
      
