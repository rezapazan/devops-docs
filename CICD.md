_***Table of Contents:***_

- [GitLab CI/CD](#gitlab-cicd)
  - [What is CI/CD?](#what-is-cicd)
    - [CD](#cd)
    - [CI](#ci)
  - [GitLab vs Others](#gitlab-vs-others)
  - [Creating .gitlab-ci.yml](#creating-gitlab-ciyml)
    - [Jobs](#jobs)
    - [Stages](#stages)
    - [External Command file](#external-command-file)
  
# GitLab CI/CD

In this file we are going to summarize the Gitlab CI/CD course.

## What is CI/CD?

### CD

We don't deploy directly to **production** environment, because we are not sure changes won't break anything, so we deploy the application in **stages** to test the application extensively on multiple levels. We deploy to **DEV** to run some tests e.g. unit tests, functional tests, lint tests, etc. Then, we deploy to **STAGING** or **PRE-PROD** environment. After some load tests & regression tests then we deploy to the **PROD**.

1. **Continues Deployment:**
   - In this method, every code change is built, tested and deployed to different environments.
   - Deployment to **PROD** happens ***automatically*** without explicit manual approval.
2. **Continues Delivery:**\
   Sometimes automated tests are not 100% reliable or there is not enough time or too difficult testing every aspect of the app, so:
   - Automatically deploy to non-production environments.
   - Approve deployment to **PROD** manually.

### CI

Developer commits code changes to remote git repo that triggers the pipeline. Each developer works with their own local copy of app code, and changes need to be **integrated ("merged")** into central shared code, so there are conflicts, tests will fail, and it will not be good for big teams.\
Developers must integrate or merge their changes back to the main branch as often as possible. Most important part is testing the application, that changes didn't break anything.

## GitLab vs Others

GitLab is an all-in-one solution that can be used as **SaaS** or **Self-Hosted** in contrast with **Jenkins** that can be used only as self-hosted. **Azure Pipelines** is another option for Microsoft environments in contrast with Jenkins & GitLab that are **open-source**.

## Creating .gitlab-ci.yml

We must use the name `.gitlab-ci.yml` as a **YAML** file to configure CI/CD for our application. This file must be located in the root folder of our app.

### Jobs

**Jobs** are the most fundamental building block of a `.gitlab-ci.yml` file, and they define what to do.

```yml
[name_for_job]:
  script:
    - [any_linux_command]
```
e.g.

```yml
run_tests:
  script:
    - echo "Running Tests..."
```

We can run some other commands before & after the script using `before_script` & `after_script`:

```yml
run_tests:
  before_script:
    - echo ""
  script:
    - echo ""
  after_script:
    - echo ""
```

Note that pipeline logic becomes part of the application code, ***Infrastructure as Code (IaC)***. Pipelines are executed on linux environments, so we can use linux commands in jobs, and we can see a full log of what is happening during the job execution.

A pipeline consists of **jobs** & **stages**, and they are the top-level component of CI/CD.

### Stages

We can group multiple jobs into stages that run in a defined order. Multiple jobs in the same stage are executed in parallel. Only if all jobs in a stage succeed, the pipeline moves on to the next stage. If any job fails, the next stage is not executed,  and the pipeline ends. We can define stages in the `.gitlab-ci.yml` file like this:

```yml
stages:
  - test
  - build
  - deploy
```

We can specify the stage for a job like this:

```yml
run_tests:
  stage: test
  script:
    - echo "Running Tests..."
```

If we want to execute jobs in a certain order within a stage it's done using `needs` keyword:

```yml
push_image:
  stage: build
  need:
    - build_image
  script:
    - echo ""
```

If we have more than 2 jobs depending on each other, we must define all the previous jobs in each step.

### External Command file
