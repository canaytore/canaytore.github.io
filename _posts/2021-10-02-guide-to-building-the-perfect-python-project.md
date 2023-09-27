---
layout: posts
classes: wide
title: "Guide to Building the ´Perfect´ Python Project"
---

In this guide, I will try to walk you through some best practices for setting up an exemplary Python project to maintain the highest quality code. Happy reading!

[Python](https://www.python.org/) is a versatile and powerful object-oriented programming language that has gained immense popularity in various domains, from web development and data science to automation and artificial intelligence. However, building a perfect Python project requires more than just writing code and make it work. Whether you're an experienced programmer or just getting started with Python, spending a tiny amount of time to setup a project with the best tools will save immense time, and lead to a happier coding experience and long-term success. 

It involves setting up a robust project structure, managing dependencies, adhering to coding standards, and ensuring efficient testing and code coverage, checking vulnerabilities or threats, and many more not covered here.

## Step 1: Project Structure

A well-organized project structure is the foundation of any Python project. A common and recommended structure is as follows:

```
my_project/
|-- src/
|   |-- __init__.py
|   |-- module1.py
|   |-- module2.py
|-- tests/
|   |-- __init__.py
|   |-- test_module1.py
|   |-- test_module2.py
|-- changelog.md
|-- Dockerfile
|-- Pipfile
|-- Pipfile.lock
|-- README.md
|-- setup.py
|-- .gitignore
```

- `src`: Houses your Python source code files reside.
- `tests`: Stores your test files.
- `changelog`: A log or record of all notable changes made to the project.
- `Dockerfile`: A text document that allow us to build images for [Docker](https://www.docker.com/).
- `README.md`: Documentation of your project, including installation instructions and usage guidelines.
- `setup.py`: Defines project metadata and dependencies.
- `Pipfile`: Manages dependencies using [Pipenv](https://pipenv.pypa.io/).
- `.gitignore`: List of files and directories to be ignored by version control. A collection of useful templates can be found [here](https://github.com/github/gitignore).

## Step 2: Dependency Management with Pipenv

[Pipenv](https://pipenv.pypa.io/) simplifies dependency management by creating a virtual environment and managing project-specific dependencies. Dependencies are external libraries or packages that your Python project relies on to function correctly. Pipenv combines the capabilities of both ´pip´ and ´virtualenv´ to provide a robust and user-friendly solution. Managing dependencies sometimes is a bit tricky, we could come across thousands of issues on StackOverFlow. 

To install Pipenv, run:

```bash
pip install pipenv
```

To initialize your project with Pipenv, run:

```bash
pipenv --python 3.7   # Use your desired Python version
```

Then, (un)install your project dependencies:

```bash
pipenv install <package-name>
pipenv uninstall <package-name>
```

Pipenv will automatically update the Pipfile and Pipfile.lock files to reflect the new dependency.

To update all dependencies to their latest versions:

```bash
pipenv update
```

To activate the Virtual Environment:

```bash
pipenv shell
```

This puts you inside the project's isolated environment. 

To leave the virtual environment and return to your system's Python, simply type:

```bash
exit
```

For best practice, I recommend you run the following to get the latest changes in the Pipfile (if any) before activating the virtualenv.

```bash
pipenv sync
```

Pipenv in general ensures a clean and isolated environment for your project, making it easy to replicate across different systems.

## Step 3: Code Formatting with isort and black

Consistent code formatting enhances readability and maintainability. [isort](https://pycqa.github.io/isort/) sorts and organizes your imports, while [black](https://black.readthedocs.io/en/stable/) formats your code according to [PEP 8](https://peps.python.org/pep-0008/) standards.

Install isort and black using Pipenv:

```bash
pipenv install isort black --dev
```

To format your code, run:

```bash
pipenv run isort src/
pipenv run black src/
```

Note that Black and Isort have incompatible default settings, so we need to customize Isort to align with Black's preferences. To do this, you should include the following in a setup.cfg file.

```yaml
[
isort
]
multi_line_output=3
include_trailing_comma=True
force_grid_wrap=0
use_parentheses=True
line_length=88
```

## Step 4: Style Enforcement with flake8

[flake8](https://flake8.pycqa.org/en/latest/) checks your code for style and potential issues. 

Install it with Pipenv:

```bash
pipenv install flake8 --dev
```

Run flake8 to check your code:

```bash
pipenv run flake8 src/
```

Address any issues and ensure your code complies with [PEP 8](https://peps.python.org/pep-0008/) guidelines.

## Step 5: Testing with pytest

Testing is crucial for ensuring the correctness of your code. [pytest](https://pytest.org/) is a popular testing framework that simplifies test writing and execution.

Install pytest using Pipenv:

```bash
pipenv install pytest --dev
```

Create test files in the `tests/` directory and write your test cases using pytest conventions. Run tests with:

```bash
pipenv run pytest
```

## Step 6: Test Coverage with pytest-cov

Code coverage helps identify areas of your codebase that lack test coverage. [pytest-cov](https://pytest-cov.readthedocs.io/en/latest/) is an excellent tool for measuring code coverage.

Install pytest-cov using Pipenv:

```bash
pipenv install pytest-cov --dev
```

To generate a coverage report, run:

```bash
pipenv run pytest --cov=src --cov-report=html
```

Open the generated HTML report in your browser to view code coverage statistics.

Preferably create a new ´.coveragerc´ file to return only the coverage statistics of our application code assuming the application code lives in the ´src´ module.

```yaml
[run]
source = best_practices

[report]
exclude_lines =
    # Have to re-enable the standard pragma
    pragma: no cover

    # Don't complain about missing debug-only code:
    def __repr__
    if self\.debug

    # Don't complain if tests don't hit defensive assertion code:
    raise AssertionError
    raise NotImplementedError

    # Don't complain if non-runnable code isn't run:
    if 0:
    if __name__ == .__main__.:
```

You may want the test coverage of application code to fail if the coverage is less than 80% by providing the ´--cov-fail-under=80´ flag.

## Step 7: Check for vulnerabilities

It analyzes your project's dependencies and display a report with any detected vulnerabilities. Follow the recommended actions to address the vulnerabilities.

To initialize the scanning, simply type:

```bash
pipenv check --output full-report 
```

## Bonus: Git hooks with pre-commit

Git hooks allow you to run scripts any time you want to commit or push. Git hook scripts are useful for identifying simple issues before submission to code review. We run our hooks on every commit to automatically point out issues in code.

Here we can configure all of tools above to run on any changed python files on committing. For that, we would need a new file called ´.pre-commit-config.yaml´.

```yaml
repos:
  - repo: repo-name
    hooks:
      - id: isort
        name: isort
        stages: [commit]
        language: system
        entry: pipenv run isort
        types: [python]

      - id: black
        name: black
        stages: [commit]
        language: system
        entry: pipenv run black
        types: [python]

      - id: flake8
        name: flake8
        stages: [commit]
        language: system
        entry: pipenv run flake8
        types: [python]
        exclude: setup.py

      - id: pytest
        name: pytest
        stages: [commit]
        language: system
        entry: pipenv run pytest
        types: [python]

      - id: pytest-cov
        name: pytest
        stages: [push]
        language: system
        entry: pipenv run pytest --cov --cov-fail-under=80
        types: [python]
        pass_filenames: false
```

If you would like to skip these hooks you can run ´git commit --no-verify´ or ´git push --no-verify´

To finish setting up, follow these steps:

```bash
# Enter project directory
cd <repo_name>

# Initialize git repo
git init

# Install dependencies
pipenv install --dev

# Setup pre-commit and pre-push hooks
pipenv run pre-commit install -t pre-commit
pipenv run pre-commit install -t pre-push
```

Once you're happy with the code you can then do your first ´git commit´ and all the hooks will be run.

## Conclusion

Here are some additional best practices to consider:

- **Version Control**: Use Git for version control and host your code on platforms like GitHub, GitLab, or Bitbucket.

- **Documentation**: Write clear and concise docstrings for your functions and classes. Use tools like Sphinx for generating documentation. Also you may have a look Documentation As Code applications.

- **Continuous Integration (CI)**: Set up a CI/CD pipeline using services like TravisCI, CircleCI, Bamboo or GitHub Actions to automate testing and deployment.

- **Linter Integration**: Integrate flake8 and pytest into your CI pipeline to enforce code style and run tests automatically on each commit.

- **Code Reviews**: Collaborate with team members through code reviews/PRs to maintain code quality and share knowledge.

- **Security**: Regularly update dependencies to patch security vulnerabilities. Using Blackduck could be an option.

Building the perfect Python project requires careful planning, adherence to best practices, and the use of modern tools. By following these steps, you can create a Python project that is clean, maintainable, well-tested, and ready for collaboration with other developers. 

Also, I personally think it would be beneficial for you to take a look at [12factor](https://12factor.net/). Happy coding!