# Project work 5

## Reflection
### Using MVVM (Model-View-ViewModel)
This week I decided to refactor the code I wrote previous week to increase the overall code quality of the project. One crucial factor for this is using the [Model-View-ViewModel](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93viewmodel) software architecture pattern. 

It works by dividing an application into three interconnected parts. The Model represents the data and business logic; the View is the user interface, showing the data; and the ViewModel acts as a link between the Model and the View, handling logic for the UI. In this setup, the ViewModel communicates with the Model to get and update data, then formats it in a way the View can easily display. Fig.3 gives a visual explanation.

<figure>
<img src="./images/week12_project/Fig3-mvvm.png" alt="Trulli" style="width:100%">
<figcaption align="center"><b>Fig.3 - MVVM</b></figcaption>
</figure>


#### Advantages of using MVVM

- **Separation of Concerns**: MVVM segregates the graphical user interface (UI) from the business logic and data model, leading to a cleaner and more manageable codebase. This separation makes it easier for developers to work on the UI and business logic independently.

- **Improved Maintainability**: By dividing the application into layers, MVVM makes it easier to maintain and update. Changes in one part of the application, such as the business logic or data model, can be made with minimal impact on the other parts.

- **Enhanced Testability**: The separation of concerns facilitates easier testing. The ViewModel can be tested separately from the UI, making unit testing more straightforward. This is especially beneficial for complex applications where testing can become cumbersome.

- **Data Binding**: Databinding automatically synchronizes data between the View and the ViewModel. This reduces the need for boilerplate code to move data in and out of the UI and ensures that the UI stays consistent with the underlying data model.

### Using the Command Pattern
Generally, using [software design patterns](https://en.wikipedia.org/wiki/Software_design_pattern) makes sense because they are like tested recipes for solving common problems in programming. They help us write code faster since we do not have to figure out everything from scratch. It is also easier to talk about our code with others because these patterns have specific names that many programmers know. Additionally, our code becomes cleaner and easier to fix or change later.

Refactoring our code to MVVM gives us the opportunity to use a desgin pattern: [Command Pattern](https://en.wikipedia.org/wiki/Command_pattern).

The Command pattern turns requests or simple operations into objects. This pattern involves a command interface, concrete commands implementing this interface, an invoker that calls the command, and a receiver that performs the action. This structure allows for flexible and extensible command-based operations in an application (Fig.4). 

<figure>
<img src="./images/week12_project/Fig4-command-pattern.png" alt="Trulli" style="width:100%">
<figcaption align="center"><b>Fig.3 - MVVM</b></figcaption>
</figure>

#### Implementing the command pattern with MVVM
In the context of MVVM, there is a special way to implement the Command Pattern. For our **ConcreteCommand** we can make use of the [ICommand interface](https://learn.microsoft.com/en-us/dotnet/api/system.windows.input.icommand?view=net-8.0) that already exists in MAUI. It is an interface that contains an *Execute* and a *CanExecute* method and it can be used within the XAML file for databinding.

The **Caller** in our implementation is the View, a command gets called by clicking the button it is bound to.

The **Receiver** is the ViewModel because it contains the action we want to execute.


#### Advantages of using Command Pattern
- **Reusability**: Commands can be reused across different parts of the UI, reducing code duplication.

- **Flexibility and Extensibility**: Changes in the business logic or user interface can be made independently without affecting each other, making the application more flexible and easier to extend.

- **Bindable User Interactions**: Commands can be easily bound to UI elements in the View, like buttons and menus, which simplifies the handling of user interactions.


### Workflow Changes
#### Increase number of Code Reviewers
We noticed that by only having one reviewer on bigger pull requests, things sometimes slip through the cracks. The minimum requirement of reviewers is still one, but now whenever there are bigger or more complex changes we try to have at least two other people review the code. The second or third reviewer is not strictly defined. How it works is that whenever someone has a bigger PR they will post it in the project group chat and ask for more people to review it. That will help prevent errors that previously went unnoticed.

#### Abstracting repeated Code
As noted in previous portfolio entries, there is a lot of repeated code due to the number of people working on the project. We have now started to coordinate better and write code that is reusable. In addition to the [AModel](https://github.com/Software-Engineering-Red/MAUI-APP/blob/develop/Models/AModel.cs) class we now also have the [ANameModel](https://github.com/Software-Engineering-Red/MAUI-APP/blob/develop/Models/ANameModel.cs) class which is a model that uses the string name as a primary key instead of an integer. There is also a class called [AResource](https://github.com/Software-Engineering-Red/MAUI-APP/blob/develop/Models/AResource.cs) encapsulating similar logic for the different resource types.

___

Week 12 is the fifth and last week in a series in which the goal is to improve your 
personal software engineering practice. Your portfolio entry has the same general content
as last week's, including:

* A descriptive summary of the issue that you worked on.
* Snippets from your code with commentary showing how you have used good software design 
  practice.
* A descriptive summary of the test code that you have written.
* A reflective summary of any changes that were requested during the code review along 
  with your fixes.
* A descriptive summary of any issues you found with the code that you were asked to review.
* A general reflective section that identifies, for example,
  * New things you have realised this week
  * Common problems that can arise in a team development situation
  * How your practice compares to other people's
  * etc.

Be sure to include links to the original items in the team's GitHub repository.

In the reflective sections this week, you should highlight ways that you persona practice
has improved as before. It would also be good to reflect on any improvements that have
been made to the agreed team workflow and related procedures. Are things working
better than they were? What further improvements could be made in the future?

## Marking

The marks for this portfolio entry will be split as follows:

* Timeliness: 5 marks (one mark will be deducted for each day or partial day that the submission 
  is late).
* Description: 1 mark is awarded for the descriptive content. You are guaranteed at least one 
  question on this entry in the final interview assessment. Please do not skimp on the 
  descriptive content. 
* Reflection: 4 marks are awarded for the quality of reflection. Again, you should think of this 
  as preparation for the interview assessment.