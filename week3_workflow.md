# Workflow

This section documents your practical work in week 3.

The main requirements are to show that:

1. You know how to use the workflow tools in GitHub
2. You have successfully carried out the required operations which are:
   * Accept a task from the project backlog
   * Update the task information appropriately
   * Update the task board appropriately
   * Complete the development task on a feature branch
   * Commit your changes with appropriate comments
   * Check your work against the Definition of Done (DoD)
   * Make a pull request

## Task workflow
#### Tasks in Github
To start working on a task we need to first pick one from the project backlog, move to "In Progress" click on it and assign the task to the appropriate user in the list to let the other developers know who is working on that task (Fig.1).

<figure>
<img src="./images/week3_workflow/Fig1-Pick_a_task.png" alt="Trulli" style="width:100%">
<figcaption align="center"><b>Fig.1 - Pick a task and assign to user</b></figcaption>
</figure>


In the issue view you can edit the description to add more information about how the task should be completed (Fig.2). Here is a link to [the issue](https://github.com/Software-Engineering-Red/MAUI-APP/issues/1).


<figure>
<img src="./images/week3_workflow/Fig2-Issue-view.png" alt="Trulli" style="width:100%">
<figcaption align="center"><b>Fig.2 - Issue View</b></figcaption>
</figure>

Now, by clicking on "create a branch" in the issue a window will pop up that allows us to genereate a feature branch on which we can start working on the implementation(Fig.3).


<figure>
<img src="./images/week3_workflow/Fig3-Creating_a_branch.png" alt="Trulli" style="width:100%">
<figcaption align="center"><b>Fig.3 - ICreating a feature branch</b></figcaption>
</figure>

#### Working on our Branch

In our local environment we can now checkout the new branch using the [Visual Studio Git Integration](https://learn.microsoft.com/en-us/visualstudio/version-control/git-with-visual-studio?view=vs-2022) or using the git bash and typing the following command.
```bash
git checkout MAUI#1
```

Now the implementation can start. We can use the the information found in the issue to ensure everything is developed according to the requirements.

To commit stage, commit and push changes we can use the Visual Studio Git Integration or the following commands.

```bash
git add *
git commit -m "Commit message"
git push
```

More information about the development workflow can be found [here](https://github.com/Software-Engineering-Red/MAUI-APP/blob/master/Documentation/workflow.md).


#### Creating a Pull Request

When all the changes are implemented we need to check them against our **Definition of Done** which can be found [here](https://github.com/Software-Engineering-Red/MAUI-APP/blob/master/Documentation/workflow.md#definition-of-done-kanban).

___
Here, you should use screenshots and descriptive commentary to show that the required
have been completed successfully.

**DO**

* Consider the layout of your content from the point of view of the reader. Many raw
  screenshots will take up a lot of space. It may be better to adopt a different strategy
  such as
  * Using thumbnails with links to full-size images
  * Combining two or more screenshots into a single image 
  * Using partial screenshots to highlight only the important information
  * etc.
* Provide links to the actual objects (e.g. task, pull request, etc.) in the team project
  in GitHub
* Read through your work to make sure that the information comes across clearly

**DON'T**

* Use humour or informal language
* Waste space with trivial or self-evident commentary
* Abbreviate your commentary using (e.g. by using bullet points). Your text should be in
  the form of grammatically correct sentences.

## Reflection

Here, you should highlight any difficulties that you faced in completing the task, and
how you resolved them.

You should also briefly discuss the current process and how it could be refined or
improved in future iterations. For example, is the DoD adequate or too onerous? Is the
current procedure for updating the task appropriate? Is the task board configured to
work in an intuitive way?
