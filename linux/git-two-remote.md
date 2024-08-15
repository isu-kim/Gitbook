# Git Two Remote

When using GitLab for private use, you also sometimes would like to upload the same code to GitHub as well. In this example, we clone a repo from GitLab and mirror the same thing to GitHub.

### 1. Cloning GitLab

For example, we have a repo in GitLab [http://git.boanlab.com/boanlab/solution](http://git.boanlab.com/boanlab/solution.git)

> This is a private repo, so you will not have an access to this.

```bash
git clone http://git.boanlab.com/boanlab/solution.git
```

### 2. Adding Remote

For example, we also have a repo in GitHub to mirror this one [https://github.com/isu-kim/boanlab-solution](https://github.com/isu-kim/boanlab-solution)

```bash
git remote add github https://github.com/isu-kim/boanlab-solution.git
```

### 3. Push GitHub

> Be aware, GitHub has base branch named `main` not `master`.

```bash
git push github main
```

This will push the code which was cloned from GitLab to GitHub.
