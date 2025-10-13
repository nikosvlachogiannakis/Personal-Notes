<!-- white.css Theme -->

# Notes

Nikos Vlachogiannakis, 2025

---

# Table of Contents

--

<div style="display: flex; text-align: left; width: 70vw; margin-left: -10vw;">

<div style="flex: 1;">

1. [<span style="color:darkred">Connect Remotely with SSH</span>](#/2)
    - [With Password](#/2/1)
    - [Passwordless Login](#/2/2)
    - [Test Passwordless Login](#/2/5)
2. [<span style="color:darkred">Create Repository on GitHub & Push Changes</span>](#/3)
    - [Option 1: Clone from GitHub](#/3/1)
    - [Option 2: Local to GitHub](#/3/2)
    - [Option 3: Using GitHub CLI (gh)](#/3/3)
    - [Token Expiry](#/3/4)
    - [Push Changes on GitHub](#/3/7)
    - [Using LazyGit](#/3/8)
    - [Skip Pre-Commit Hooks](#/3/9)
    - [Git LFS Notes](#/3/10)
    - [GitHub Connection Check](#/3/11)
    - [Additional Information](#/3/12)

</div>

<div style="flex: 1;">

3. [<span style="color:darkred">Create a Project with Poetry</span>](#/4)
    - [Option 1: Use Poetry on Existing Project](#/4/2)
    - [Option 2: Create New Project with Poetry](#/4/3)
    - [Additional Information](#/4/7)
4. [<span style="color:darkred">Advanced Workflow</span>](#/5)
    - [Organized Python Project with pre-commit, formatters & linters](#/5/1)
    - [Documentation with Sphinx](#/5/10)
    - [Additional Information](#/5/13)

</div>

<div style="flex: 1;">

5. [<span style="color:darkred">Reveal.js Presentation</span>](#/6)
    - [Install Node.js](#/6/1)
    - [Get a Template](#/6/2)
    - [Editing Slides](#/6/3)
    - [Customizing Theme](#/6/4)
    - [Slide Transitions](#/6/5)
    - [Change Markdown File](#/6/6)
    - [Load Slides Locally](#/6/7)
    - [Hosting on GitHub Pages](#/6/8)
    - [GitHub Pages Troubleshooting](#/6/9)
    - [Export to PDF](#/6/10)
    - [Managing Large Files with Git LFS](#/6/11)
    - [Additional Information](#/6/13)

</div>

</div>

--

## Extra

<div style="display: flex; gap: 4em; text-align: left;">

<div style="flex: l;">

6. [<span style="color:darkred">Git Commands Cheat Sheet</span>](#/7)

7. [<span style="color:darkred">Store Screenshots as PDF</span>](#/8)
    - [Windows Methods](#/8/1)
    
    - [Linux Methods](#/8/2)

</div>

<div style="flex: l;">

8. [<span style="color:darkred">Cheat Sheet Reference</span>](#/9)
    - [File & Directory Management](#/9/1)
    - [File Viewing & Editing](#/9/2)
    - [Search & Navigation](#/9/3)
    - [System Utilities](#/9/4)
    - [Package Management](#/9/5)
    - [Python & pyenv](#/9/6)
    - [SSH & Remote Access](#/9/7)
    - [FTP & File Transfer](#/9/8)
    - [Shortcuts](#/9/9)
    - [Git Essentials - Part 1](#/9/10)
    - [Git Essentials - Part 2](#/9/11)


</div>

</div>

---

# How to connect to a Remote Machine?

--

# With Password

<pre><code class="language-bash" data-trim>
ssh user@remote-server
</code></pre>


Then you must add the password of the remote machine and you are all set!

***Note***: The first time you connect, SSH may ask you to confirm the server’s authenticity — type `yes` and press Enter.

--

# Passwordless Login

--

## 1. Generate an SSH key(If you don't have one)

Locally, on your computer, open Terminal(if you have Windows open *Git Bash*)  and write:

<pre><code class="language-bash" data-trim>
ssh-keygen -t ed25519 -C "your_email@example.com"
</code></pre>

- Press Enter to accept the default path: C:\Users\username\.ssh\id_ed25519

- Choose no passphrase to enable passwordless login

## 2. Copy the Public Key to the Remote Server

<pre><code class="language-bash" data-trim>
ssh-copy-id user@remote-server
</code></pre>

--

## 3. Use SSH Config File
Create or edit:
<pre><code class="language-bash" data-trim>
~/.ssh/config
</code></pre>
Add:
<pre><code class="language-bash" data-trim>
Host myserver
  HostName REMOTE_SERVER
  User USERNAME
  IdentityFile ~/.ssh/id_ed25519
</code></pre>
Now you can simply run:
<pre><code class="language-bash" data-trim>
ssh myserver
</code></pre>

--

## 4. Test the Passwordless SSH Login
Run:
```bash
ssh user@remote-server
```
OR
```bash
ssh myserver
```

You should be logged in without a password.

### Log Out From The Remote Machine

Press <span style="color:blue">Ctrl + d</span> or <span style="color:blue">exit</span>

---

# Create a Repository on GitHub (3 Options)
# & Push Changes

--

# Option 1. Create a repo on GitHub and clone it locally

- Create a Repository on GitHub (either public or private) <span style="color:lime">with a README.md file</span>
- Go to the path where you want to have your project:
<pre><code class="language-bash" data-trim>
cd path
</code></pre>
- Copy the link of the SSH key (ssh_key) from your repository in GitHub and write the following in terminal:
<pre><code class="language-bash" data-trim>
git clone ssh_key
</code></pre>
- You can open the file we cloned in VS Code and work on it.

--

# Option 2. Create a File locally and push to GitHub

- Go to GitHub and create a repo. Name it the same as your local repo and <span style="color:red">do NOT create a README.md</span> (you already have one locally). You want your repo to be completely empty.
- Once the repo is created, go to Code → SSH and copy the repo SSH link.
- Locally, on your cmd/bash do:

<pre><code class="language-bash" data-trim>
git init
</code></pre>

<pre><code class="language-bash" data-trim>
git add .
</code></pre>

<pre><code class="language-bash" data-trim>
git commit -m "Initial commit"
</code></pre>

<pre><code class="language-bash" data-trim>
git remote add origin ssh_link-you-copied
</code></pre>

<pre><code class="language-bash" data-trim>
git branch -M main
</code></pre>

<pre><code class="language-bash" data-trim>
git push -u origin main
</code></pre>

Your commits should be pushed to GitHub, and your local repo is connected to the GitHub repo.

--

# Option 3. Using GitHub CLI (gh)

This option uses <span style="color:cyan">HTTPS protocol</span> instead of SSH.

- Install [GitHub CLI](https://github.com/cli). After successful installation of `gh`, it asks for a token. Follow the link and create a token with the proposed permissions. After this action, you can use `gh` to do things in GitHub.
- Connect it to your GitHub with the following command and follow the prompts
<pre><code class="language-bash" data-trim>
gh auth login
</code></pre>
- To create a new repo in the current folder, do:
<pre><code class="language-bash" data-trim>
git init
git add .
git commit -m "Initial commit"
gh repo create REPO_NAME --public --source=. --push
</code></pre>
- Clone a repo:
<pre><code class="language-bash" data-trim>
gh repo clone repo_name
</code></pre>
- See all your repos in GitHub:
<pre><code class="language-bash" data-trim>
gh repo list
</code></pre>

--

# Important Note: Token Expiry

If that token <span style="color:red">expires</span> you need to run the following to make sure the token is updated:

##### In the remote machines' Terminal:
<pre><code class="language-bash" data-trim>
gh auth login
</code></pre>

##### Then it will ask:
<pre><code>
? Where do you use GitHub?
</code></pre>
Select:
<pre><code>
GitHub.com
</code></pre>

Continue... $\Downarrow$

--

##### Then it will ask:
<pre><code>
? What is your preferred protocol for Git operations on this host?
</code></pre>
Select:
<pre><code>
HTTPS
</code></pre>

##### Question:
<pre><code>
? Authenticate Git with your GitHub credentials?
</code></pre>
Select:
<pre><code>
Yes
</code></pre>

Continue... $\Downarrow$

--

##### Question:
<pre><code>
? How would you like to authenticate GitHub CLI?
</code></pre>
Select:
<pre><code>
Paste an authentication token
</code></pre>

##### Then on the following:
<pre><code>
? Paste your authentication token:
</code></pre>
Paste the token you (re-)generated and copied from GitHub.

Then it will say that you are logged in as `account_name` or something similar.

--

# Push changes on GitHub

<pre><code class="language-bash" data-trim>
cd repository_name
</code></pre>

<pre><code class="language-bash" data-trim>
git status
</code></pre>

With this command, we can see all changed files which are <span style="color:red">red</span>.

So, we use:
<pre><code class="language-bash" data-trim>
git add (all files on red separated by space)
</code></pre>

<span style="color:red">OR</span>

<pre><code class="language-bash" data-trim>
git add .
</code></pre>

Then:
<pre><code class="language-bash" data-trim>
git commit -m "This is a message explaining what changes we made"
</code></pre>

Finally:
<pre><code class="language-bash" data-trim>
git push
</code></pre>

Now, all the changes you made are uploaded in your repository on GitHub!

--

# Otherwise, Use LazyGit

After the <span style="color:lime">successful installation</span> of LazyGit:

- Inside the folder you want to push, write:
<pre><code class="language-bash" data-trim>
lazygit
</code></pre>

- A new window will appear where, in the upper left side, all the files that have changes will be displayed. In order to include each file in your push, press <span style="color:blue">Spacebar</span>.

- After selecting all the files you want to push, press <span style="color:blue">c</span> and write what changes you have made, then press <span style="color:blue">Enter</span>.

- To push these changes to your GitHub, press <span style="color:blue">Shift+P</span>.

- To exit LazyGit, press <span style="color:blue">q</span>.

--

# Skip Pre-Commit Hooks

Use this when you intentionally want to skip automated checks like linting or formatting hooks:

<pre><code class="language-bash" data-trim>
git status
</code></pre>

<pre><code class="language-bash" data-trim>
git add .
</code></pre>

<pre><code class="language-bash" data-trim>
git commit -m "a message" --no-verify
</code></pre>

<pre><code class="language-bash" data-trim>
git push --no-verify
</code></pre>

That way the pre-commit does ***not*** run and it pushes the changes just fine and it does not ask you for username or password.

--

# Git LFS Notes

- When you have large files (e.g., `.mp4` files) that are stored in git-lfs and try to clone a repository from GitHub, you have to install and initialize Git LFS and then run:
<pre><code class="language-bash" data-trim>
git lfs pull
</code></pre>

- Otherwise, if you want to replace the video, make sure to do it before running `git add`, using a proper `.mp4` file (e.g., downloaded from the web).

--

# GitHub Connection Check

If we want to check if GitHub is connected with our machine using ssh:
<pre><code class="language-bash" data-trim>
ssh -T git@github.com
</code></pre>

You should get a message like:
<pre><code class="language-bash" data-trim>
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
</code></pre>


--

# Additional Information

## What is SSH?

<span style="color:blue">SSH (Secure Shell)</span> is a cryptographic network 
protocol that allows you to securely connect to remote computers or servers over an 
unsecured network. It's commonly used for accessing and managing servers, transferring 
files, and authenticating with services like GitHub. SSH works by using a key pair: a 
private key stored on your device and a public key shared with the remote service. When 
you connect, SSH verifies your identity using these keys, ensuring encrypted, secure 
communication without needing to send passwords.

## What is gh?

<span style="color:blue">gh</span> is Github's command line interface (CLI) that can 
be used to manage your github without leaving the command line and the keyboard.

--

## What is LazyGit?

<span style="color:blue">Lazygit</span> is a simple, fast, terminal-based UI for Git.

It lets you manage your Git repositories more easily than typing out commands - you can 
stage, commit, push, pull, resolve conflicts, and browse logs interactively, all from a 
clean text interface.

It’s especially useful when you want the power of Git without memorizing all the 
commands, and it works entirely in your terminal without needing a graphical IDE.

## IDE(Integrated Development Environment)

<span style="color:blue">IDE</span> is a software application that provides a set of 
tools for programmers to write, test, and debug their code more efficiently - all in one
 place. 

Examples of IDEs: PyCharm, Visual Studio, Eclipse.

---

# Create a Project with Poetry

--

## $\circ$ Download [Poetry](https://python-poetry.org/docs/)

After downloading Poetry you can either use it on an existing project or create a project from scratch.

--

## Option 1. Use [Poetry](https://python-poetry.org/docs/) On An Existing Project

### $\circ$ Open Terminal and go Into Your Project Directory

```bash
cd my_project
```

### $\circ$ Initialize Poetry

```bash
poetry init
```

- **Creates or Updates** *pyproject.toml* in the current directory.
- It asks you questions (name, version, description, dependencies, etc.) and writes them into the file.
- Does **not** create folders like `src/`, `tests/`, or `__init__.py`.
- Used when:
  - You already have an existing Python project, and just want to manage it with Poetry.
  - You don’t want Poetry to scaffold a whole new layout.

--

## Option 2. Create a New Project with Poetry From Scratch

### $\circ$ Open Terminal and run the following command

```bash
poetry new my_project
```

This creates:

```markdown
my_project/
├── my_project/
│   └── __init__.py
├── tests/
│   └── __init__.py
│   └── test_my_project.py
├── pyproject.toml
```

### $\circ$ Move Into the Project Directory

```bash
cd my_project
```

--

## Next Steps (After Initializing or Creating a Project)

### $\circ$ Open in VS Code

```bash
code .
```

### $\circ$ Set Up the Python Environment

Inside the project folder:

Install dependencies *without* activating the shell:
```bash
poetry install
```

Then *activate* the virtual environment:
```bash
poetry shell
```

--

### $\circ$ Configure VS Code to Use Poetry's Environment

VS Code needs to know about Poetry’s virtual environment.

#### Automatically:

When you run ***poetry shell***, then start VS Code using ***code .*** from the same terminal - it usually detects it.

#### Manually:

1. Press ***Ctrl+Shift+P*** (Command Palette)  
2. Select: ***Python: Select Interpreter***  
3. Choose the one inside ***.venv*** or the one Poetry created in cache.

--

### $\circ$ Add Packages or Dependencies

Add a package (e.g. requests):

```bash
poetry add requests
```

Add a dev dependency (e.g. pytest):

```bash
poetry add --dev pytest
```

## Exit the Poetry shell

Write:
```bash
Ctrl+D
```
OR
```bash
exit
```

--

# Additional Information

--

## What is Poetry?

<span style="color:blue">Poetry</span> is a tool for managing Python projects. It 
helps you declare, install, and resolve dependencies, build your package, and publish 
it. Unlike using pip and virtualenv separately, Poetry handles both dependency 
management and virtual environments seamlessly.

It’s widely used because it creates a pyproject.toml file (modern Python standard), 
ensures reproducible builds, and makes it easy to develop and distribute Python 
packages.

--

### Comment
**Dev dependencies** are packages that are only needed during the development phase of a 
project—such as testing frameworks, linters, formatters, or documentation tools. We use 
them to support writing, testing, and maintaining code without including them in the 
final production environment, which helps keep deployments lightweight, secure, and 
efficient.

--

### Dependency vs. Package

These words overlap, but they’re used slightly differently:
- **Package**
    - A Python library/module you can install and import.
    - Example: `requests` is a package on PyPI.
    - You can think of it as the *unit of distribution*.
- **Dependency**
    - A package your project _depends on_ to run or develop.
    - Defined in your `pyproject.toml`.
    - Example: if your project uses `requests` for HTTP calls, then `requests` is one of your dependencies.

So:
- Every **dependency** is a **package**, but not every package in existence is *your* dependency.
- Some dependencies can themselves have dependencies (called **transitive dependencies**). Poetry handles all of that and records exact versions in `poetry.lock`.


--

## TOML File

A <span style="color:blue">.toml</span> file (short for Tom’s Obvious, Minimal 
Language) is a simple configuration file format that is easy for humans to read and 
write.

In Python projects - especially when using tools like Poetry - the pyproject.toml file 
is used to define the project’s metadata, dependencies, build settings, and tool 
configurations.

---

# Advanced Workflow

--

# Create a Well-Organized Python Project Using Poetry

Create a well-organized Python project using Poetry, adding tools to improve code quality, debugging, interactivity and documentation, while keeping dependencies cleanly grouped into dev and docs environments.

--

## On Terminal:

1.  Go inside the projects' path (which has a .toml file, if you don't have a project follow the steps on Poetry slides to create one and then you can skip steps 1. and 2.)

```bash
cd project_path
```

2. **Initialize** Poetry

```bash
poetry init
```

**Continue $\downarrow$**

--

3.  **Create** a `.pre-commit-config.yaml` File

This file is required for `pre-commit` to function properly. It specifies which code checks (known as "hooks") should be executed before each commit. These hooks can include tasks like formatting code, running linters, checking for trailing white space, or verifying file permissions.

- Create this file in the **root** of your project (same level as `pyproject.toml`)

```bash
touch .pre-commit-config.yaml
```

- Then open it in your editor (e.g., VS Code)

```bash
code .pre-commit-config.yaml
```

**Continue $\downarrow$**

--

- Paste the following:

```bash
repos:

-   repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v5.0.0
    hooks:
    -   id: trailing-whitespace
    -   id: end-of-file-fixer
    -   id: check-yaml
    -   id: check-toml
    -   id: check-added-large-files
    -   id: check-case-conflict
    -   id: check-executables-have-shebangs
    -   id: check-shebang-scripts-are-executable
    -   id: check-merge-conflict

-   repo: https://github.com/macisamuele/language-formatters-pre-commit-hooks
    rev: v2.14.0
    hooks:
    -   id: pretty-format-yaml
        args: [--autofix, --indent, '4', --preserve-quotes]
    -   id: pretty-format-toml
        args: [--autofix]

-   repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.11.13
    hooks:
    -   id: ruff-format
        args: [--config=pyproject.toml]
    -   id: ruff-check
        args: [--config=pyproject.toml, --fix, --unsafe-fixes, --output-format=concise, --quiet]

-   repo: https://github.com/asottile/pyupgrade
    rev: v3.20.0
    hooks:
    -   id: pyupgrade
        args: [--py310-plus]

-   repo: https://github.com/dannysepler/rm_unneeded_f_str
    rev: v0.2.0
    hooks:
    -   id: rm-unneeded-f-str

-   repo: https://github.com/numpy/numpydoc
    rev: v1.8.0
    hooks:
    -   id: numpydoc-validation
        exclude: '^(sandbox/|^docs/)'

-   repo: local
    hooks:
    -   id: mypy
        name: mypy
        entry: mypy
        args: [--show-error-codes, --explicit-package-bases, --config-file=pyproject.toml]
        language: system
        types: [python]
        require_serial: true
        exclude: ^(sandbox|docs)/

-   repo: local
    hooks:
    -   id: pytest
        name: pytest
        entry: pytest
        args: [-W, "ignore::DeprecationWarning", -s]
        language: system
        types: [python]
        require_serial: true
        exclude: ^(sandbox|docs)/

```

**Continue $\downarrow$**

--

4. **Download and add** ***pre-commit*** - a tool that automatically runs checks like formatting or linting before you commit code - into your project using Poetry. Pre-commits can be used to ensure code quality by running linters, formatters and other checks on the codebase. The ***--group dev*** flag tells Poetry not to include ***pre-commit*** in your project's main dependencies. Instead, it places it under `[tool.poetry.group.dev.dependencies]` in your ***pyproject.toml*** (***pre-commit = "^x.y.z"***). This means it's used only during development and is not needed by users who install your package in production.

```bash
poetry add --group dev pre-commit
```

### Why use this?

- Keeps your production environment clean
- Makes it easy to separate tools like formatters, linters, and test runners
- Supports installing only specific groups:
  - ***poetry install --with dev*** $\rightarrow$ includes dev tools
  - ***poetry install --without dev*** $\rightarrow$ skips them for lightweight builds

**Continue $\downarrow$**

--

5. **Install** ***bpython***, a fancy interactive Python REPL (alternative to python), as a dev tool.

```bash
poetry add --group dev bpython
```

6. **Install** ***pretty-errors***, a package that makes Python tracebacks more readable.

```bash
poetry add --group dev pretty-errors
```

7. **Activate pretty-errors for the current terminal session**. Makes Python errors and tracebacks prettier.

```bash
python3 -m pretty_errors
```

In the options that will appear, press ***1*** for the first and ***2*** for the second.

**Continue $\downarrow$**

--

8. **Install** everything

```bash
poetry install
```

9. **Activate the Poetry virtual environment**, where you can run installed packages (like pre-commit) directly.

```bash
poetry shell
```

### OR

Activate the same virtual environment manually. This is redundant if you've already used ***poetry shell***.

```bash
.../project-...-py3.11/bin/activate
```

**Continue $\downarrow$**

--

10. **Initialize Git and connect with GitHub** (you need this for *pre-commit* later)

```bash
git init
echo ".venv/" >> .gitignore
echo "__pycache__/" >> .gitignore
echo "*.pyc" >> .gitignore
```

```bash
git add .
```

```bash
git commit -m "Initial commit"
```

```bash
git remote add origin ssh_link_of_repo
```

```bash
git branch -M main
```

```bash
git push -u origin main
```

11. **Install the Git hook** (***.git/hooks/pre-commit***). After this, every git commit
 will automatically run the checks defined in ***.pre-commit-config.yaml***.

```bash
pre-commit install
```

**Continue $\downarrow$**

--

12. **List the pre-commit Git hook file**, confirming it has been installed. (*Optional*)

```bash
ls .git/hooks/pre-commit
```

13. **Show the contents of the pre-commit hook file** (a shell script that runs pre-commit). (*Optional*)

```bash
cat .git/hooks/pre-commit
```

14. **Run all configured pre-commit hooks on all files**, not just changed ones. It is useful for validating the entire project upfront.

```bash
pre-commit run --all-files
```

**Continue $\downarrow$**

--

# Create Documentation using Sphinx

***Sphinx*** is a documentation generator, most famous in the Python ecosystem.

### What it does:
You write your docs in reStructuredText (***.rst***) or Markdown, and Sphinx converts them into HTML websites, PDFs, ePubs, man pages, etc.

--

15. **Install documentation-related tools**:
    - ***myst-parser***: for Markdown support in Sphinx
    - ***numpydoc***: parses Numpy-style docstrings
    - ***sphinx***: the main documentation generator
    - ***sphinx-math-dollar***: support for math in Markdown using `$...$` for inline, `$$...$$` for block
    - ***sphinx-rtd-theme***: the Read the Docs theme for docs

```bash
poetry add --group docs myst-parser numpydoc sphinx sphinx-math-dollar sphinx-rtd-theme
```

16. Initialize Sphinx

```bash
sphinx-quickstart docs
```

17. **Enter the**  ***docs/***  **directory**.

```bash
cd docs
```

```bash
make clean
```

**Continue $\downarrow$**

--

18. **Build the Sphinx documentation in HTML format** using a custom Makefile target (***html-all***).
If this is a custom Makefile, the command could:
- Build all doc pages
- Run validation
- Include Markdown and reStructuredText files

```bash
make html-all
```

--

# Additional Information

--

# What is pre-commit?

<span style="color:blue">pre-commit</span> is a framework for managing and running 
Git hooks, especially the <span style="color:blue">pre-commit</span> hook - a script 
that runs right before you make a commit. It helps enforce code quality by running tools
 (like linters, formatters, or security checks) automatically before code is committed 
to your repository.

--

# Why use pre-commit?

## Benefits:

- **Auto Formatting**: Automatically runs tools like 
<span style="color:blue">black</span>, <span style="color:blue">isort</span> or 
<span style="color:blue">prettier</span> before commits.

- **Catch Errors Early**: Run linters like <span style="color:blue">flake8</span>, 
<span style="color:blue">mypy</span>, or <span style="color:blue">eslint</span> 
before code gets into version control.

- **Consistent Code Style**: Enforces uniform code style across your team or projects.

- **Avoid Forgetting**: Automates things you might forget to do manually (e.g., remove debug
 prints, fix import order).

- **Prevent Bad Commits**: Can block commits that violate rules (e.g., large files, TODOs 
left in code).

--

# Common Tools Managed with pre-commit

- <span style="color:blue">black</span> – auto-format Python code

- <span style="color:blue">flake8</span> – lint Python code

- <span style="color:blue">mypy</span> – static type checker

- <span style="color:blue">isort</span> – sort imports

- <span style="color:blue">detect-secrets</span> – prevent committing secrets

- <span style="color:blue">trailing-whitespace</span>, 
<span style="color:blue">end-of-file-fixer</span>, etc. – small but useful 
consistency checks

--

# How pre-commit works

When you install and configure <span style="color:blue">pre-commit</span>, it creates
 a Git hook at <span style="color:blue">.git/hooks/pre-commit</span>. When you run 
<span style="color:blue">git commit</span>, it automatically:

1. Runs configured tools on your changed files

2. Fails the commit if any tool fails

3. Lets the commit succeed if everything passes

--

# Linters

Analyze the code for syntax, style and potential bugs. Catch issues like:

- Unused variables
- Wrong import order
- Missing docstrings

# Formatters

Automatically reformat code to follow a consistent style.

- Identation
- Quote style
- Line breaks
- Comma placements

Linters and formatters get their config from 
<code class="language-toml" style="background-color: #3d3d3dff; color: #fff; max-height: none; overflow: visible;" data-trim>pyproject.toml</code>

--

# Example Configuration

<pre><code style="max-height: none; overflow: visible;" class="language-toml" data-trim>
[tool.ruff]
exclude = ["docs"]
line-length = 88
target-version = "py311"

[tool.ruff.lint]
ignore = [
  # pylint
  # No pylints
  # ruff
  "RUF001",
  # flake8-bandit
  "S101",
  ...
  "ISC001",
  "N806",
  "N803",
  "SLF001"
]
select = ["ALL"]

[tool.ruff.lint.flake8-quotes]
inline-quotes = "double"

[tool.ruff.lint.pydocstyle]
convention = "numpy"
</code></pre>

--

# NumPy Docstring Example

NumPy docstrings follow a specific format to ensure consistency and clarity. Here's a 
basic template for writing NumPy-style docstrings:

<pre><code class="language-python" data-trim>
def example_function(param1, param2):
    """
    Brief summary of the function.

    Extended description of the function, which can cover
    multiple lines and provide more detailed information.

    Parameters
    ----------
    param1 : int
        Description of the first parameter.
    param2 : str
        Description of the second parameter.

    Returns
    -------
    bool
        Description of the return value.

    Raises
    ------
    ValueError
        If `param1` is not a positive integer.

    See Also
    --------
    related_function : Description of related function.
    
    Notes
    -----
    Additional notes about the function, its usage, or implementation details.

    Examples
    --------
    >>> example_function(3, 'test')
    True
    """

    if param1 <= 0:
        raise ValueError("param1 must be a positive integer")
    # Example function logic
    return True
</code></pre>


--
## What is Python REPL (Read-Eval-Print Loop)

It's an interactive Python shell you get when you run:

<pre><code class="language-bash" data-trim>
python
</code></pre>

You can:

- Type code line by line
- See results instantly
- Test, debug etc.

### Better REPLs:

- bpython - colorful, autocompletion
- ipython - powerful, rich features

--

# YAML File

A <span style="color:blue">.yaml</span> file is a human-readable text file used to 
store configuration or structurred data. It uses indentation to represent hierarchy and 
is common in tools like Docker, GitHub Actions and pre-commit. It's simple, clean and 
easy for both humans and machines to read.

--

# .rst File

An <span style="color:blue">.rst</span> file (reStructuredText) is a plain-text 
markup format commonly used in Python projects for writing documentation.

- Works seamlessly with Sphinx, which converts <span style="color:blue">.rst</span> 
into HTML, PDF, ePub, etc.
- More powerful than Markdown for large docs: supports cross-references, indexes, 
advanced tables, math, and extensible directives
- Standard for many Python libraries because PyPI supports .rst for project descriptions

### Example

<pre><code class="language-typescript" data-trim>
Project Title
=============

Features
--------

* Fast
* Easy

.. code-block:: python

   print("Hello, World!")
</code></pre>

--

# What is Traceback?

A <span style="color:blue">traceback</span> is the error message Python shows when 
your code crashes. It includes a list of function calls that led to the error, showing 
where and why it happened. It helps you debug by pointing to the exact line and file 
that caused the exception.

---

# Reveal.js Presentation

--

## 1. Install Node.js

You need Node.js (which includes npm) installed on your system.

**Windows / macOS / Linux:**

- Visit: [nodejs](https://nodejs.org/)
- Download the LTS version and install it

- To verify:
<pre><code class="language-bash" data-trim>
node -v
npm -v
</code></pre>

--

## 2. Get a Template

- Go to a template on GitHub
- Click Code -> Download ZIP
- Extract it on your computer
- Rename the folder as you wish

--

## Editing Your Slides

Slides are written in slides.md using Markdown.

Use **---** to separate horizontal slides, and **--** for vertical slides:

<pre><code class="language-markdown" data-trim>
# Welcome

&#45;&#45;&#45;

## Slide 1

Some text

&#45;&#45;

- Vertical slide 1

&#45;&#45;

- Vertical slide 2
</code></pre>

--

# Customizing Theme

Reveal.js themes are in the dist/theme folder.

To change the theme, edit index.html and modify this line:

<pre><code class="language-html" data-trim>
<link rel="stylesheet" href="dist/theme/black.css" id="theme">
</code></pre>

You can try other themes like <span style="color: blue;">white.css</span>, 
<span style="color: blue;">night.css</span>, <span style="color: blue;">
moon.css</span>, etc.

--

# Transition Between Slides

### Option 1: Set Globally in the Reveal Configuration (index.html)

<pre><code class="language-html" data-trim>
<script>
  Reveal.initialize({
    transition: 'fade', // or 'slide', 'convex', 'concave', 'zoom', 'none'
    transitionSpeed: 'default' // 'fast', 'slow', or 'default'
  });
</script>
</code></pre>

### Option 2: Per Slide with data-transmition in the Markdown File

<pre><code class="language-bash" data-trim>
&lt;!-- .slide: data-transition="zoom" --&gt;
</code></pre>

### Or for vertical slides

<pre><code class="language-bash" data-trim>
&lt;!-- .slide: data-transition="fade-in slide-out" --&gt;
</code></pre>

--

# Change Markdown File

Inside the file that contains the template, you can create a folder called 
<span style="color:blue">Presentations</span> for example. There you can store all 
Markdown files for every presentation you make.

#### To change the presentation, you can do the following:

- Go to index.html
- Find:
<pre><code class="language-html" data-trim>
data-markdown="slides.md"
</code></pre>
- Change it to:
<pre><code class="language-html" data-trim>
data-markdown="Presentations/notes.md"
</code></pre>

- Now, the presentation written in the notes.md file 
will appear.

--

# Load Slides Locally

- Inside the folder, open a terminal and run:
<pre><code class="language-bash" data-trim>
npm install
</code></pre>
This installs local development tools like the server (gulp).

- Run the Presentation:
<pre><code class="language-bash" data-trim>
npm start
</code></pre>
- Open your browser at:
<pre><code class="language-bash" data-trim>
http://localhost:8000
</code></pre>
You should see your slides rendered.

--

# Hosting Slides on GitHub Pages

You can publish your presentation online for free using GitHub Pages.

#### **Step-by-Step:**

1. Make sure your repository includes:
    - index.html
    - slides.md
    - dist/ and plugin/ folders from Reveal.js
    - empty .nojekyll file(prevents GitHub from using Jekyll)
2. Go to your GitHub repository -> Settings -> Pages
3. Under Build and Deployment, set:
    - Source: Deploy from branch
    - Branch: main
    - Folder: / (root)
    - Click Save
4. Wait 1-10 minutes. GitHub will show a <span style="color:green">green message</span> 
like the following:
<pre><code class="language-markdown" data-trim style="color:green;">
Your site is live at https://your-username.github.io/repo-name/
</code></pre>

--

## If the green message does 
## <span style="color:red">NOT</span> appear

- Add a .nojekyll file(empty file with that name)
- Make a small commit to trigger a rebuild:
<pre><code class="language-markdown" data-trim>
git commit --allow-empty -m "Trigger rebuild"
git push
</code></pre>

- Visit your slides live in the browser!

## Note:

- GitHub Pages do <span style="color:red">not</span> work for private repos unless you
 are on a GitHub Enterprise plan.

- You will see a 404 or the site won’t build.

--

### Export Slides to PDF

- Reveal.js can export slides to PDF:
<pre><code class="language-bash" data-trim>
npm install -g decktape
</code></pre>
<pre><code class="language-bash" data-trim>
decktape reveal http://localhost:8000 slides.pdf
</code></pre>

- Useful for email, offline viewing, or submissions.

--

# Managing Large Files with Git LFS

If you need to include large binary files in your presentation (e.g. .mp4 videos, .pdf 
documents), it is recommended to use Git Large File Storage (LFS) instead of committing 
them directly to the repository.

## Install Git LFS

- Ubuntu/Debian:
<pre><code class="language-bash" data-trim>
sudo apt install git-lfs
</code></pre>

- Windows:  
Download and install from [git-lfs.com](https://git-lfs.com/)

**Continue $\downarrow$**

--

### Set Up Git LFS in Your Repo

1. Run this ince per machine:
<pre><code class="language-bash" data-trim>
git lfs install
</code></pre>
2. Track the file types you want to store with LFS:
<pre><code class="language-bash" data-trim>
git lfs track "*.mp4"
git lfs track "*.pdf"
</code></pre>
3. Add and commit:
<pre><code class="language-bash" data-trim>
git add .gitattributes
git add media/my-video.mp4
git commit -m "Add video using Git LFS"
git push
</code></pre>

--

# Additional Information

--

## Git LFS

- GitHub’s free LFS plan includes 1 GB of storage and 1 GB/month bandwidth.
- You can view tracked LFS files using: <span style="color:lime">
<code class="language-bash" data-trim>git lfs ls-files</code></span>

To avoid LFS entirely, you can also upload media externally (e.g. Google Drive or a web 
server) and link to them in your slides.


--

## What is Reveal.js?
<span style="color:blue">Reveal.js</span> is an open-source framework for creating 
HTML-based presentations using Markdown or HTML. Instead of writing slides in a visual 
editor (like Keynote or PowerPoint), you write them in plain text - and Reveal.js turns 
them into a modern, interactive slideshow in the browser.

--

## HTML File

- The <span style="color:blue">HTML</span> file is the main entry point of the 
presentation.

- It defines the page structure, loads Reveal.js and its plugins, configures settings, 
and optionally contains the slides directly if not using Markdown.

--

## CSS File

- The <span style="color:blue">CSS</span> file defines the styling of your 
presentation.

- It customizes the look and feel - fonts, colors, layout - either by overriding the 
default Reveal.js theme or adding new styles.

--

## Markdown File

- The <span style="color:blue">Markdown (.md)</span> file contains the slide content 
written in simple, readable Markdown syntax.

- It’s loaded by the HTML file (via the Reveal.js markdown plugin), making it easy to 
edit slides without touching HTML.

--

## JSON File

- A <span style="color:blue">JSON</span> file is optional and is used to store extra 
data or configuration for the presentation.

- It can define metadata, timing, or slide-related data that JavaScript in the HTML file
 can read and use.

---

# Git Commands

--

<div style="display: flex; gap: 0.5em; text-align: left;">
<img src="figures/git_cheatsheet_page_1.jpg" alt="Git Page 1" width="550" height="650">
<img src="figures/git_cheatsheet_page_2.jpg" alt="Git Page 2" width="550" height="650">
</div>

--

<div style="display: flex; gap: 0.5em; text-align: left;">
<img src="figures/git_cheatsheet_page_3.jpg" alt="Git Page 3" width="550" height="650">
<img src="figures/git_cheatsheet_page_4.jpg" alt="Git Page 4" width="550" height="650">
</div>

---

# Store screenshot as PDF instead of PNG to NOT lose quality

--

## Windows

### Option 1

- Take a screenshot using <span style="color:blue">Win + Shift + S</span>
- <span style="color:blue">Paste</span> it into Paint or Word or a 
<span style="color:blue">browser</span>
- Got to <span style="color:blue">File</span> > 
<span style="color:blue">Print</span> > choose <span style="color:blue">Microsoft 
Print to PDF</span> as the printer
- <span style="color:blue">Save</span> the PDF

### Option 2 (PowerShell)

<pre><code class="language-bash" data-trim>
Start-Process -FilePath "screenshot.png" -Verb PrintTo -ArgumentList "Microsoft Print to PDF"
</code></pre>

--

## Linux

### Option 1

- Use <span style="color:blue">PrtSc</span> or a screenshot tool like 
<span style="color:blue">Flameshot</span>
- Open the image in <span style="color:blue">LibreOffice Draw</span> or 
<span style="color:blue">GIMP</span> or a <span style="color:blue">browser</span>
- <span style="color:blue">Export</span> or print to PDF:
  - In LibreOffice: <span style="color:blue">File</span> > 
  <span style="color:blue">Export As</span> > 
  <span style="color:blue">Export as PDF</span>
  - In GIMP: <span style="color:blue">File</span> > 
  <span style="color:blue">Export As</span> > 
  <span style="color:blue">select PDF</span>

### Option 2 (Terminal)

<pre><code class="language-bash" data-trim>
convert screenshot.png screenshot.pdf
</code></pre>

---

# Cheat Sheet

--

## File & Directory Management
| Command | Description |
| --- | --- |
| ls | List files in current directory |
| ls -la | List all files with details |
| cd folder_name | Change directory |
| cd | Go to home directory |
| cd .. | Go up one directory |
| cd - | Go to previous directory |
| mkdir folder_name | Create a new folder |
| rm file | Delete file |
| rm -rf folder | Force delete folder and contents |
| cp source dest/ | Copy file/folder |
| mv old new | Move or rename file/folder |
| touch file.txt | Create empty file |

--

## File Viewing & Editing
| Command | Description |
| --- | --- |
| cat file.txt | Show file contents |
| less file.txt | Scrollable view of file |
| nano file.txt | Edit file in nano editor |
| code . | Open folder in VS Code (if installed) |

--

## Search & Navigation
| Command | Description |
| --- | --- |
| find . -name '*.py' | Find all `.py` files |
| grep "pattern" file.txt | Search for text in a file |
| grep -r "text" folder/ | Recursive search in folder |
| pwd | Show current directory |
| history | View command history |

--

## System Utilities
| Command | Description |
| --- | --- |
| whoami | Show current user |
| df -h | Show disk usage |
| free -h | Show memory usage |
| top / htop | Process monitor (install `htop`) |
| uptime | System uptime |

--

## Package Management (APT)
| Command | Description |
| --- | --- |
| sudo apt update | Refresh package index |
| sudo apt upgrade | Upgrade installed packages |
| sudo apt install pkgname | Install package |
| sudo apt remove pkgname | Remove package |

--

## Python & pyenv
| Command | Description |
| --- | --- |
| python3 --version | Show system Python version |
| pyenv install 3.11.13 | Install specific Python version |
| pyenv global 3.11.13 | Set global Python version |
| pyenv local 3.11.13 | Set version only for current folder |
| pyenv versions | List installed versions |
| pyenv which python | Show path to Python binary |
| poetry init | Create new Poetry project |
| poetry env use $(pyenv which python) | Link Poetry to pyenv Python |
| poetry env list | Lists all exiting environments |
| poetry env remove <env> | Removes poetry environment |
| poetry add <package> | Add dependency |
| poetry install | Install all libraries in toml |
| poetry shell | Activate project virtual environment |

--

## SSH & Remote Access
| Command | Description |
| --- | --- |
| ssh user@host | Connect to remote machine |
| ssh -i ~/.ssh/id_rsa user@host | Use specific SSH key |
| ssh-keygen -t ed25519 | Generate new SSH key |
| cat ~/.ssh/id_ed25519.pub | Show public key to add to GitHub/remote |
| scp file user@host:/remote/path | Copy file to remote server |
| scp user@host:/remote/file ./local/ | Copy file from remote server |

--

## FTP & File Transfer

| Command | Description |
|--------|-------------|
| ftp host | Connect to an FTP server |
| sftp user@host | Connect securely via SFTP |
| scp file user@host:/path | Copy file to remote server |
| scp user@host:/file ./ | Copy file from remote server |
| rsync -avz file user@host:/path | Sync file/directory over SSH |
| rsync -avz user@host:/path ./ | Download file/directory via rsync |
| mput *.txt | Upload multiple files in FTP |
| mget *.txt | Download multiple files in FTP |
| put file.txt | Upload file in FTP |
| get file.txt | Download file in FTP |
| bye | Exit FTP session |

--

## Shortcuts
| Command | Description |
| --- | --- |
| Ctrl + C | Cancel current command |
| Ctrl + D | Exit terminal or virtual env |
| Ctrl + L | Clear terminal screen |
| ↑ / ↓ | Navigate command history |
| Tab | Autocomplete files or commands |
| !! | Repeat last command |
| Ctrl+ | Move by word instead of character |

--

## Git Essentials - Part 1
| Command | Description |
| --- | --- |
| git init | Initialize a new Git repository |
| git config --global user.name/email | Set global Git username/email |
| git clone <url> | Clone a remote repository |
| git status | Check status of working directory |
| git add <file> | Stage changes for commit |
| git add . | Stage all changes |
| git diff | Shows unstaged changes |
| git diff --staged | Shows staged changes |
| git diff HEAD | Shows changes since last commit |
| git diff commit1 commit2 | Shows changes between commits |
| git diff branch1 branch2 | Shows changes between branches |
| git commit -m 'msg' | Commit staged changes with message |
| git commit --amend | Edit and replace the most recent commit. |

--

## Git Essentials - Part 2

| Command | Description |
| --- | --- |
| git log | View commit history |
| git branch | List branches |
| git checkout <branch> | Switch to branch |
| git checkout -b <branch> | Create and switch to new branch |
| git merge <branch> | Merge branch into current |
| git pull | Fetch and merge from remote |
| git push | Push local commits to remote |
| git remote -v | List remote connections |
| git restore  <file> | Restore a file to its last commited state |
| git reset --soft HEAD~1 | Undo the last commit, keep changes staged |
| git stash | Temporarily save uncommited changes |
| git stash pop | Apply and remove the most recent stash |

---

# That is all!
