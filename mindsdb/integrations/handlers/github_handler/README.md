# GitHub Handler

GitHub handler for MindsDB provides interfaces to connect to GitHub via APIs and pull repository data into MindsDB.

---

## Table of Contents

- [GitHub Handler](#github-handler)
  - [Table of Contents](#table-of-contents)
  - [About GithHub](#about-githhub)
  - [GitHub Handler Implementation](#github-handler-implementation)
  - [GitHub Handler Initialization](#github-handler-initialization)
  - [Implemented Features](#implemented-features)
  - [TODO](#todo)
  - [Example Usage](#example-usage)

---

## About GithHub

GitHub is a web-based hosting service for version control using Git. It is mostly used for computer code.
It offers all of the distributed version control and source code management (SCM) functionality
of Git as well as adding its own features. It provides access control and several collaboration
features such as bug tracking, feature requests, task management, and wikis for every project.

## GitHub Handler Implementation

This handler was implemented using the [pygithub](https://github.com/PyGithub/PyGithub) library.
PyGithub is a Python library that wraps GitHub API v3.

## GitHub Handler Initialization

The GitHub handler is initialized with the following parameters:

- `repository`: a required name of a GitHub repository to connect to
- `api_key`: an optional GitHub API key to use for authentication
- `github_url`: an optional GitHub URL to connect to a GitHub Enterprise instance

Read about creating a GitHub API key [here](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token).

## Implemented Features

- [x] GitHub Issues table for a given Repository
  - [x] Support LIMIT
  - [x] Support WHERE
  - [x] Support ORDER BY
  - [x] Support column selection

## TODO

- [ ] GitHub Pull Requests table for a given Repository
- [ ] GitHub Commits table for a given Repository
- [ ] GitHub Releases table for a given Repository
- [ ] GitHub Contributors table for a given Repository
- [ ] GitHub Branches table for a given Repository

## Example Usage

The first step is to create a database with the new `github` engine. The `api_key` parameter is optional,
however, GitHub aggressively rate limits unauthenticated users. Read about creating a GitHub API key [here](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token).

~~~~sql
CREATE DATABASE mindsdb_github
WITH ENGINE = 'github',
PARAMETERS = {
  "repository": "mindsdb/mindsdb",
  "api_key": "your_api_key",    -- optional GitHub API key
};
~~~~

Use the established connection to query your database:

~~~~sql
SELECT * FROM mindsdb_github.issues
~~~~

Run more advanced queries:

~~~~sql
SELECT number, state, creator, assignee, title, labels
  FROM mindsdb_github.issues
  WHERE state="all"
  ORDER BY created ASC, creator DESC
  LIMIT 10
~~~~
