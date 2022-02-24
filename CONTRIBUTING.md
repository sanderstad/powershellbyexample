## Contributing

Thanks for your interest in contributing to PowerShell by Example!

* To contribute to the project you need to have a GitHub account. You can then clone the repository and start working on the project.

* We're open to adding more examples to the site. They should be on things used by many programmers and only require the standard library. If you're interested in adding an example, _please open an [Discussion](https://github.com/sanderstad/powershellbyexample/discussions) to discuss the topic first_.

* If you have a question or want to discuss the project, please go to the [Discussion](https://github.com/sanderstad/powershellbyexample/discussions).

* We're not going to change the navigation of the site, in particular adding a "previous section" link or an "index" link other than the one present on the page.

## Adding a new post

To add an item you need to add a `.md` file to the `content/post` directory.
Make the file name represent the part that you want to add like `arrays.md` or `functions.md`.

A post should at least have this content at the top:

```
---
title: [title]
author: [your name]
date: 'yyy-MM-dd'
categories:
  - Example
weight: [weight]
---

[description of the post]
```

* The title should be short and describe the content.  
* The author can be your full name.  
* The date should be in the format `yyy-MM-dd`, i.e. `2020-01-01`.
* The weight should be a number higher than the weight in the latest post incremented by 10. If the weight of the last post is 120, your weight should be 130 if you want it to be the latest post.

After you've added the post you can run `git add .` and `git commit -m "Add [title] post"` to commit the changes.

After you've committed the changes to your clone you can then create a pull request (PR) from your clone to the main branch of this project.

The PR will be assessed by the maintainers of the project and if it is approved your changes will be merged into the main branch.

This step will automatically start the action to publish the content.

## Code

PowerShell code should be wrapped in a markdown powershell code block "```powershell".

## Results

If you want to display the results of your example code, you have to put the entire output into a "```" block

## Short codes

Hugo works with short codes and in this project we created a couple of short codes to help out.

### Errors

If you want to add an error message to your post you can use the `error` short code like so:

```
{{< error >}}
THIS IS AN ERROR
{{</ error >}}
```

A good example of using this short code is in the `variables.md` file.