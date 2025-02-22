
# Botpress

Welcome to the Botpress Workshop, organized by MTC and ACM-W! 🎉

Whether you're a complete beginner or have some experience, this workshop is designed to help you grasp the fundamentals of Botpress and kickstart your journey in chatbot development. By the end of the session, you'll have the skills to build your own interactive chatbot from scratch. Let's dive in and start building! 🚀


## What is Botpress?

Botpress is an open-source, low-code platform for building, managing, and deploying chatbots. It provides a visual interface to design chatbot workflows and integrates Natural Language Understanding (NLU) to process user inputs effectively.

Botpress is designed to be highly customizable and can be used to create AI-powered conversational agents for various applications, including customer support, automation, education, and interactive assistants.

## Account Setup and Bot creation 

Follow these simple steps to get started with bot building

- Visit [Botpress cloud](www.botpress.com) and hit sign up
- Sign up with your Google Account and verify your account
- You have now reached your workspace from where you can manage your chatbots
- Click "Create bot", then name your bot with a simple name and click Open in Studio
- You will be asked to choose a template. Ignore all the cool looking templates and click on Quick Start, as we will be building from scratch!
- Look for `Edit Main Flow` at the bottom of the page and click it

![image](https://github.com/user-attachments/assets/b08f0c52-a182-4fae-b60f-f63990778172)




What you see on your screen now is Botpress Studio, where you'll be designing the workflow of your bot

## Some basic terms

Here are some basic terms to know before getting started

**Workspace** - The main environment in Botpress Studio, where you manage all your chatbots. It includes tools for building, testing, and deploying bots.

**Workflow** - A structured sequence of nodes and transitions that define how a chatbot interacts with users. It represents the chatbot's conversation logic. It is basically what is going on behind the scenes of your bot

**Nodes** - The building blocks of a workflow, each representing a step in the conversation. Nodes can contain messages, conditions, API calls, or actions. All nodes in your workflow will be between two nodes, start node and end node.

**Cards** - Elements inside a node that define specific actions, such as sending messages, capturing user input, or triggering integrations.

Now that you know these, lets start building!!

# Travel Website Chatbot

For this workshop, we will be building a chatbot for a travel blog. Firstly, go over to the Autonomous node in the middle of the screen. Right click on it and click delete node. The remaining parts of this documentation is divided into sections, each named after the node we are using in the workflow. This workflow will contain the logic that is going on behind the scenes. We advice you to keep testing your bot from time to time in the Emulator panel on the right hand side.

## Introduction Node

Right click anywhere on the studio, and click Standard node:
![image](https://github.com/user-attachments/assets/577ac5e9-f695-4c60-bc99-61f20a0cca6b)

Move over to the "Standard1" text on your node and rename the node to "Introduction". This is the first building block of your bot.
Join the start node to this introduction node with the help of the dots next to these nodes

Click on Add card within the node, and you will see a variety of cards that you can add to the node:

![image](https://github.com/user-attachments/assets/92d995af-34cf-4c93-8c84-80e9676629c0)

Click on "Text" card under the "Send Message" section. Once you do this, you will see a box where you can type the message you want to send to the user.
Type an introductory statement - "Hi, this is John, your travel agent for today!"

Idea of this Bot - We have to ask the user the size of the group they are travelling with, and based on the size of the group, we advise them the place they should go to.
- To capture the group size, click on Add card, scroll down to the "Capture Information" section in the card library and click on "Number". 
- Now next to the bot, in the card information panel for this Number card, locate the "Question to the user" field and Enter the Question : "How big of a group are you travelling with?"
- And right below this text input, you will see the "Select/Create a Variable" option. Click on it, name this variable as "groupSize" and click create variable.

What has happened now is, the bot will ask the user for the size of the group they are traveling with and store the user input in a variable called workflow.groupSize. We can use this variable to help the bot decide the place the user has to travel to

To keep this simple, if the group size is less than 5, the bot advises the user to go to Singapore; if group size is more than 5 and less than 10, the user goes to Hawaai and if the group size is more than 10, the user goes to New Zealand.

To do this, click on add card, locate the "Flow Logic" section, and add three Expression cards
Expression cards allow you to redirect the user from one node to another based on some condition

- For the first Expression card, in the card information panel, under the "Label" field, write "if group size is less than 5".
  Then in the condition panel, click on the blue icon to disable Generative AI for this field, and write the following code snippet
  ```Javascript
  workflow.groupSize < 5
  ``` 
- For the second expression card, add the label - "If group size greater than 5 and less than 10", and add the following code snippet in the "Condition Field":
   ```Javascript
   workflow.groupSize  >  5 && workflow.groupSize < 10
  ```
- For the third expression card, add the label - "If group size greater than 10", and add the following code snippet in the "Condition Field":
   ```Javascript
   workflow.groupSize  >  10
  ```

## Destination Nodes

Create a standard node by right clicking anywhere on the Botpress Studio and click Standard Node.

Rename this node as "Singapore", which was the destination to go to if the groupSize was less than 5. From Introduction Node, join the dot from the first expression card to this "Singapore" Node

Similarly, create two more nodes, "Hawaii" and "New_Zealand" and join them accrodingly. After these changes, your workflow should look like this:

![image](https://github.com/user-attachments/assets/dc88bfb2-3d3b-41de-9559-9a7788ec4657)

Now, within Singapore Node, add a text card from the "Send Message" section. Write the following message to be sent 

```
Your group size is @workflow.groupSize , for a group this size, we advice you to go to Singapore
```

The `@workflow.groupSize` helps to print the value stored in our groupSize variable

Do the same for the "Hawaii" and "New_Zealand" nodes. 

Now, once we have advised the user about the place they should go to, we always ask them if they have any questions. For this, we are going to create another Standard node called "Question" (will be discussed in the upcoming section). But before that we will create expression cards, as we did before, in all the Destination nodes. We will set the condtion in these expression nodes as "true", and these expression cards will be connected to the new "Question" node that we are going to make.

So with this, once the bot is done suggesting the place to visit, it will always redirect the user to the question node behind the scene.

## Question Node

After connecting the expression cards from the destination nodes, the workflow should look like this:

![image](https://github.com/user-attachments/assets/8766642b-686a-478f-88f7-60f46a6f986a)

Now we ask the user if they have any questions, if yes, we take them to another node, and if not, we end the session.
For this, click on "Add Card" in Question node, and add the "Single Choice" card under the "Capture information" section. 

In the "Question to the user" field of this card, write - "Do you have any questions?"

And in the "Choices" section of this card, add two items - "Yes" and "No"

Once you do this, you will see that the questions node has one "Single Choice" card with two options, Yes and No. 
As discussed earlier, if the user does not have any questions, we end the session. So for this, simply connect the "No" to the end node.

However, if the user still has some questions, we need to have a **special arrangement** to answer those questions. 
Firstly, we are going to create a new node and name it "Answering_Questions". 
We then connect the "Yes" option from the Questions_Node to this new "Answering_Questions" Node.

## Answering Questions (The node with the special arrangement)

Create another Standard node and rename it "Answering_Questions". Connect the "Yes" from the previous node to this node. 

For our bot to answer questions, it needs to have information stored in some sort of database. The bot can look for answers in this database and give the answer to the user. This special arrangement is called "Knowledge Base".

To add a knowledge base to your bot, click on "Knowledge Bases" on the left hand side of your studio, and click on `New Knowledge Base` and name it "Travel Bot"

Now, within the list of Knowledge Bases, click on Travel Bot. This is what it should look like:
![image](https://github.com/user-attachments/assets/9f4d7c56-a0f0-4a44-b2f1-a75f9605daad)

As you can see here, we can make our bot retrieve information from multiple sources, like a text document, a website, or by simply doing a web search. Since we want our bot to answer only Travel based questions, we are going to click on `Web Search`, and select `Search on specific websites` under the `On which websites should we search on` field.

For this exercise, add the following link to the `Websites to search on` field:
```
https://theworldtravelguy.com/
```

So our `Travel bot` Knowledge base will always refer to this website for the information it needs. We can add other sources too, but this should be enough for this exercise.

Now, before going back to building our bot, Click "Agents" on the left of your Botpress Studio, and under "Knowledge Agents", disable the "Answer Manually" option.

![image](https://github.com/user-attachments/assets/dd16cf60-d31b-4dba-8bcc-8371862b2ed0)

Now, to return to your bot, click "Workflow" on left panel.

In the Answering_Questions Node, add another card called "Raw Input" which can be found in the "Capture Information" section. 
- Add a question under the "Question to the user" field.
- And in the Knowledge Base section, select the "Travel Bot" we just created!

And now, our bot will look for answers to the user's question from the knowledge base we just created. Once the question has been answered, we want to ask the user again if they have any more questions. So for this, we add another expression card to the node, with the condition set to true. This expression card will be connected to the "Questions" node we created earlier. 

However, there is a possibility that the bot is unable to find an answer from the knowledge base. In this case, the bot will throw an error and we need to handle it safely. For this, we create another expression card, we label this card as "Error Handling", and write the following code snippet:
**Note** - Make sure to add this "Error Handling" expression card ABOVE the "always" expression card we created prior to this. You can drag cards within a node and keep them in the order you want.
```Javascript
!turn.KnowledgeAgent.responded
```
Make a new standard node, name it "Error", and connect the "Error Handling" expression card to this new node.

![image](https://github.com/user-attachments/assets/2d4a597c-db82-4c4c-9c63-77cf576b3294)

## Error Node

Within this node, add a new text card from the "Send Message" section and write the following text:
"I am sorry, I cannot answer the question"
This message will help us handle the error safely

Following this, we want the bot to ask the user if they have any more questions
For that, we add another expression card, with the condition set to "true", and connect this card to the Question Node

## Summary of workflow

Our bot's workflow finally looks like this

![image](https://github.com/user-attachments/assets/7eb6033e-3df8-48b6-ae98-b6742517f1a9)

Let's summarise the workflow:
- Step 1
  Firstly, the bot asks the user about the size of the group
  - If the group size is lesser than 5, we go to Singapore
  - If the group size is greater than 5 and lesser than 10, we go to Hawaii
  - If the group size is greater than 10, we go to New Zealand

- Step 2
  After giving this information, the bot asks the user if they have any questions
  - If Yes, we ask the question (By going to Answering_Questions Node)
  - If No, we end the session

- Step 3
  In the Answering_Questions Node, once the user asks the question, there are two possibilities :
  - If the bot answers the question, we ask the user if they have more questions, by going to the Questions Node
  - If the bot cannot answer the question, we go to the Error node

- Step 4
  Within the error node, the bot says that it cannot answer the question. After sending this message, the bot goes back to the Question Node to ask the user if they have any queries. 

Steps 2-4 are repeated till the user has no questions to ask
