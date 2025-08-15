
### \#\# Step-by-Step Workflow

Here is the standard, step-by-step process to set this up and maintain it.

#### 1\. Clone the Forked Repository

* **Clone Your Fork:** On your local machine, clone your newly created fork (not the original). Replace `YOUR_USERNAME` with your GitHub username.
    ```bash
    git clone https://github.com/YOUR_USERNAME/n8n.git
    cd n8n
    ```

#### 2\. Configure an "Upstream" Remote

Next, you need to tell your local repository where the official n8n code lives so you can pull updates from it.

  * **Add the Upstream:** In your terminal, add a new remote named `upstream` that points to the original n8n repository.
    ```bash
    git remote add upstream https://github.com/n8n-io/n8n.git
    ```
  * **Verify:** You can check that it was added correctly. You should see `origin` (your fork) and `upstream` (the official repo).
    ```bash
    git remote -v
    ```

#### 3\. Create Your Customization Branch

**This is the most important step.** Never make your custom changes directly on the `master` or `main` branch.

  * **Create a New Branch:** Create and switch to a new branch for your UI modifications. Give it a descriptive name.
    ```bash
    git checkout -b my-ui-changes
    ```
  * **Make Your Changes:** Now, you can start modifying the UI code (the Vue.js components, styles, etc.).
  * **Commit Your Changes:** As you work, commit your changes to this branch as you normally would.
    ```bash
    git add .
    git commit -m "feat: customize login page logo"
    ```

-----

### \#\# The Update Workflow: Pulling in New Versions

This is the process you will follow every time you want to update your custom version of n8n with the latest official changes.

#### 1\. Fetch the Latest Official Changes

First, get all the new commits from the `upstream` (official) repository.

```bash
git fetch upstream
```

#### 2\. Update Your Local Master Branch

Switch to your local `master` branch and merge the changes from the official `upstream/master` into it. Your local `master` branch should now be a perfect mirror of the official one.

```bash
git checkout master
git merge upstream/master
```

#### 3\. Rebase Your Custom Branch

Now, apply your custom changes on top of the newly updated code.

  * **Switch back to your custom branch:**
    ```bash
    git checkout my-ui-changes
    ```
  * **Rebase onto master:** This command rewinds your custom commits, updates the branch with the latest code from `master`, and then replays your commits one by one on top of the new code.
    ```bash
    git rebase master
    ```

#### 4\. Resolve Conflicts (If Necessary)

During the `rebase`, you may encounter **merge conflicts**. This happens when the official n8n code has changed in the exact same place that you made a modification.

  * Git will pause the rebase and tell you which files have conflicts.
  * Open these files and you will see markers (`<<<<<<<`, `=======`, `>>>>>>>`) indicating the conflicting changes.
  * Edit the files to manually merge the code, keeping the parts you need from both versions.
  * After resolving the conflicts in a file, stage it: `git add <filename>`.
  * Once all conflicts are resolved, continue the rebase: `git rebase --continue`.

After the rebase is successful, your `my-ui-changes` branch will contain all the latest n8n updates plus your unique modifications. You can then proceed to build the project and deploy it.

-----

### \#\# Important Considerations

  * **⚠️ Merge Conflicts are Inevitable:** Be prepared to handle them. The more you customize, the more likely they are to occur, especially if the n8n team refactors a part of the UI you've edited.
  * **Maintenance Overhead:** This workflow requires ongoing effort. Every time you want to update, you must repeat the update process.
  * **Breaking Changes:** The n8n team might introduce breaking changes to the frontend architecture that fundamentally clash with your modifications, requiring you to rework your custom code significantly.
