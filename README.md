# Working with Big/Challenging Data Collections

## How to contribute

### Outline of steps to contribute
1. Open an issue with any updates you think are needed. This is helpful to keep track of changes being made to the book.
2. Make the updates/edits (see steps below) as a PR to the base repo.
3. Assign someone to review your PR (see steps below). 
4. Merge the PR once it has been approved.

#### Steps to make edits/updates to this book
- **Fork the base repository.** You can do this by clicking "Fork" in the upper right corner of the window. This will clone (i.e. copy) the base repo into your own account, so you should now have a repo at [your-gh-username]/BigData that is identical to the base repo.
- **Make the edits or updates in your own fork.** You can now update the content of your forked version of /BigData. Depending on preference, you can create a new branch in your forked repo and make all updates within your new branch, or you can update the main branch directly. Edits can be made in a number of ways:
  - From your forked repo, type the "." key to open a VSCode editor directly in your browser. You can make edits/additions and commit them directly to your forked repo.
  - You can clone the base repo to your local computer (see instructions [here](https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository)) and use whatever editor you choose to make edits. If you do this, it is recommended that you add two remote repos by setting "origin" to your forked repo and "upstream" to the base repo (see instructions for adding remote repo [here](https://docs.github.com/en/get-started/getting-started-with-git/managing-remote-repositories)). 
- **Open a pull request (PR) to the base repo.** GitHub has made this easy for us with an option just above the list of files in our forked repo. You should see a banner that says something like "This branch is 1 commit ahead of ACDguide:main." To the right, there is a drop-down button to "Contribute" and "Create pull request". Click that and it takes you to a page that shows the updates you have made. Then click "Create pull request" and give the PR a title and description if needed. Then click "Submit" and your updates will show up in the "Pull Requests" tab of the base repo! To link your PR with its corresponding issue, type "Resolves #[issue-number]", and this will automatically close the issue that this PR resolves. If your PR does not resolve, but is related to an issue, then you can simply reference the issue by typing "#[issue-number]".

<div class="alert alert-primary" role="alert">
  No edits should be made to the main branch of ACDguide/BigData (the base repository), unless correcting minor typos.
</div>

#### Reviewing/commenting on PRs
- Assign someone to review your PR. This can be done by clicking the gear in the upper right of the PR next to "Assignees". A drop-down menu of all maintainers should appear and you can choose the appropriate person or people.
- Who should you ask to review your PR? It depends on the page you are updating.
  - `Methods of data storage`: Claire (@hot007)
  - `Methods of accessing data`: Paige (@paigem)
  - `Data formats and metadata`: Chloe (@chloemackallah)
  - `Computations with large datasets`: Scott (@ScottWales)
  - `Methods of data storage`: Alica and Paola (@AliciaTak and @paolap)
  - any other pages, for structural changes, or if you are unsure who to assign: Paige (@paigem)
You can also assign multiple reviewers if you think your PR would benefit from other/more reviewers.


## How to use the JupyterBook locally

### Building the book

If you'd like to develop on the Working with Big/Challenging Data Collections book locally, you will need to build the book on your local machine. You should:

- Clone this repository and run
- Run `pip install -r requirements.txt` (it is recommended you do this within a virtual environment)
- (Recommended) Remove the existing `BigData/_build/` directory
- Run `jupyter-book build BigData/`

A fully-rendered HTML version of the book will be built in `BigData/_build/html/`.

### Hosting the book

The html version of the book is hosted on the `gh-pages` branch of this repo. A GitHub actions workflow has been created that automatically builds and pushes the book to this branch on a push or pull request to main.

If you wish to disable this automation, you may remove the GitHub actions workflow and build the book manually by:

- Navigating to your local build; and running,
- `ghp-import -n -p -f BigData/_build/html`

This will automatically push your build to the `gh-pages` branch. More information on this hosting process can be found [here](https://jupyterbook.org/publish/gh-pages.html#manually-host-your-book-with-github-pages).

## Contributors

We welcome and recognize all contributions. You can see a list of current contributors in the [contributors tab](https://github.com/ACDguide/BigData/graphs/contributors).

## Credits

This project is created using the excellent open source [Jupyter Book project](https://jupyterbook.org/) and the [executablebooks/cookiecutter-jupyter-book template](https://github.com/executablebooks/cookiecutter-jupyter-book).
