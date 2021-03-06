---
title: Local To-Dos for Developers
date: 2017-12-11T22:49:12.000Z
tags: [git, to-do, github]
description: My approach to managing local To-dos
cover_image: ./cover.png
cover_description: https://git.io/todos
---

A few days ago I started a small, pet project with a goal of automating the way I manage To-Dos. It’s basically a Command Line Interface (CLI) that helps to create/remove/mark/finish To-Dos.

In this post, I will try to outline how I manage local To-Dos before and after I built the tool. While my approach isn’t unique and doesn’t fit all projects/developers, it may somehow help or inspire others ;)

## Local To-Dos? Exactly!

The “To-Do” term may refer to different meaning in different contexts. In this case, I’m referring to small tasks you are expected to finish them within few hours.

However, even for those “small” tasks, you may manage them in varies ways. Some people prefer to use an [Issue Tracking System (ITS),](https://en.wikipedia.org/wiki/Issue_tracking_system) while others prefer to write To-Dos code comments even if some [people think](http://wiki.c2.com/?TodoCommentsConsideredHarmful) it’s an [AntiPattern](https://en.wikipedia.org/wiki/Anti-pattern). And course there is a group of people those just write their To-Dos in a simple [plain text](http://todotxt.org) file.

I don’t think there is “one-thing-fits-all” solution, so currently, I’m using them all, and here is how:

- I use ITS solutions (i.e. [GitHub Issues](https://guides.github.com/features/issues/)) for things that require attention from the whole team members, or those need discussion with other people (i.e. Bugs, Feature Requests). To-Dos stored in an ITS usually have a higher priority than others specified elsewhere.
- I write To-Do code comments if I think the issue requires attention from all team members, but not NOW. They aren’t urgent and don’t break anything right now. I sometimes use an extension for my editor to highlight those comments, but also use bots like [Jason Etcovitch](https://github.com/JasonEtco) ‘s [awesome bot](https://github.com/apps/todo) for GitHub to automatically creates GitHub issues corresponding to code comments.
- Also, I usually maintain a TODO.md file locally for each repository to keep track of my progress (e.g. To-Dos of To-Dos 😝) as I usually work on multiple things at the same time, and sometimes it happens to not be able to work on a project for about a few weeks, so I needed a way to keep track of my uncompleted work. I don’t commit that file of course and it’s always [ignored by Git](https://git-scm.com/docs/gitignore).

The tool I’ve built focuses on automating the last part- the “Local” To-Dos!

## Features set

I called my tool “Git Todos” as it depends on Git being installed to work. However, it’s not a [Git-Flow](http://nvie.com/posts/a-successful-git-branching-model/)-like CLI and doesn’t assume any workflow.

Generally, the tool works by simply maintaining a local .todos.yml file on each repository which is actually a replacement for my old hand-written TODO.md . In addition to the sweet syntax of [YAML](https://en.wikipedia.org/wiki/YAML), it’s also easier to parse for tools (i.e. like this one). You can freely directly write things to the .todos.yml if you want, but that what I built the tool for ;)

The tool itself doesn’t (yet) help much regarding the prevention of accidentally pushing the .todos.yml file. But, it’s handy to add a .gitignore entry for now!

### Managing To-Dos:

Adding To-Dos are simple as you might have imagined. For example, here is how to add a To-Do:

{{< figure src="./adding-todos.png" caption="Adding To-Dos" >}}

> **NOTE:** for most commands, you may pass -s or --simple to simplify the process if possible, for add command, the tool simply won’t prompt for “description” field.

Now if you list your To-Dos it may look like this:

{{< figure src="./list-todos.png" caption="List available To-Do items" >}}

Trivial, huh?

Also, the tool provides other — expected — commands such as show which displays all To-Do item details like below. But, the tool doesn’t support “Edit” command, because I think it’s easier to open the .todos.yml in any editor and edit stuff.

It also worth mentioning that the tool doesn’t (and probably won’t ) support any kind of prioritizing/ coloring/ labeling …etc, it doesn’t help you to organize To-Dos but rather get them done!

### Marking To-Dos

Marking To-Dos makes it even easier to use the tool, for example, if you want to remove a To-Do item, the command will show a selection list like below to let you select which item to delete:

{{< figure src="./remove-an-item.png" caption="Selection list to remove an item" >}}

If you want to remove a marked item you can pass -m to the command to skip selection step. You may only mark single To-Do :)

### Importing issues

This is actually a cool thing!

The tool enables you to import issues (typically the title, description and number) from [https://github.com](https://github.com) (Enterprise edition too, at least theoretically) and use them as local To-Do items. The tool is smart enough to filter the result down to only issues in your repository. To determine your repository URL, we read git configurations (remotes to be specific) and extract the repository info from the origin URL. Here is a demo:

{{< figure src="./import-todos.png" caption="Importing To-dos"  >}}

Great! And you may also use an [advanced search syntax](https://help.github.com/articles/searching-issues-and-pull-requests/) such as label:"in progress" 🙌

When you import issues from GitHub the tool will also store the corresponding issue number to use it later when you finish that To-Do.

Did we just say “Finish”?

### Finishing To-Dos

The last part of the puzzle is to actually finish/close an item. For that purpose, the tool provides a finish command.

Just like any To-Do application, we need a way to see your work history, but unlike other solutions, the only way to show your work history is to use git log (it’s what it has been built for anyway)!!!

As you might have guessed, finish command actually commits staged files and removes the To-Do item. Not just that, it also helps you to follow the [Conventional Commits](https://conventionalcommits.org/lang/en) guidelines, take a look:

{{< figure src="./finishing-a-todo-part-1.png" caption="Finishing a To-Do (Part 1)" >}}
{{< figure src="./finishing-a-todo-part-2.png" caption="Finishing a To-Do (Part 2)" >}}

The tool automatically sets some defaults values for you and let you modify them if you need. For example, it automatically set the “Short Description” of the commit to the Todo’s title and adds the issue number (if any) to the list of issues to be closed by this action (the commit!)

And here we go, let’s check our history:

{{< figure src="./git-history.png" caption="Work history ;)"   >}}

### A little encouragement while you work

Finally, In case you didn’t notice, there are some one-line encouragement messages on the last line of some commands output, e.g. “Nice work”, “I see what you did there” ..etc. The feature is inspired by “[encourage](https://github.com/Haacked/encourage-atom)”, the popular Atom editor package.

## Wrap up

If you like the tool you can download the binary from the official [GitHub repository](https://github.com/z0al/git-todos) (you can also use this sweet, short URL [https://git.io/todos](https://git.io/todos))

Feedback is really appreciated ❤
