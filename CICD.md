***Table of Contents:***

- [GitLab CI/CD](#gitlab-cicd)
  - [Useful Links](#useful-links)
  - [What is CI/CD?](#what-is-cicd)
    - [CD](#cd)
    - [CI](#ci)
  - [GitLab vs Others](#gitlab-vs-others)
  - [Creating .gitlab-ci.yml](#creating-gitlab-ciyml)
    - [Jobs](#jobs)
    - [Stages](#stages)
    - [External Command file](#external-command-file)
    - [Workflow Rules](#workflow-rules)
    - [Self-defined Variables](#self-defined-variables)
  - [GitLab Architecture](#gitlab-architecture)
    - [Executors](#executors)
  
# GitLab CI/CD

In this file we are going to summarize the Gitlab CI/CD course.

## Useful Links

- [GitLab CI/CD Course](https://downloadly.ir/elearning/video-tutorials/complete-gitlab-ci-cd-course-with-docker-kubernetes-microservices/)
- [Predefined Variables](https://docs.gitlab.com/ee/ci/variables/predefined_variables.html)

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

We can list commands that we want to execute in the **script** section, but it would be unreadable if there are too many of them. To overcome this issue, we can create an **external bash file**, and write commands in it. Then, we can use that file as an input to script section.

Bash file:
```bash
pwd
ls
mkdir test-data
ls
```

YAML file:
```yml
run_commands:
  script:
    - chmod +x bash_file.sh
    - ./bash_file.sh
```
**Note:** each pipeline is executed on a a fresh linux environment. There is a **GitLab server** that is the main component of the architecture and manages the pipeline execution. The jobs are run on **GitLab runners**.

### Workflow Rules
 
By default, pipeline is triggered automatically for all branches, but we don't want to release new app from feature or bug fixes branch. Instead, we want to run tests to validate our code. This is done by using `only` keyword:

```yml
build_image:
  only:
    - main
  stage: build
  script:
    - echo ""
```
We can use `except` to define when a job does not run.

We can define workflow rules to control when and for what branch is pipeline executed. It can prevent copying `only` & `except` for each job.

```yml
workflow:
  rules:
    - if: $CI_COMMIT_BRANCH != "main" && $CI_PIPELINE_SOURCE != "merge_request_event"
      when: never
    - when: always
```

**Note:** `rules` can also be used on job level as a replacement for `only` & `except`.
**Note:** `$CI_COMMIT_BRANCH` is an **environment variable** predefined by GitLab. You can find all predefined variables in the [provided link](#useful-links).

We want to run the tests pipeline for **merge requests**, but deployment and building the app is only for main branch, so we added `$CI_PIPELINE_SOURCE` to the `if` statement.

**Note:** we must add `only: - main` to `build` & `deploy` jobs, so that those jobs are only run for the main branch.

### Self-defined Variables

**Use Case:** we want to run pipeline for every microservice, and we want to pass the microservice name to the pipeline as a parameter (or the name of the target environment). The value is not hardcoded in the `.gitlab-ci.yml` file, so it is more reusable.

We can add variables from settings in GitLab.
**Note:** It's the standard way to define variable names in Upper-Case e.g. `DEPLOYMENT_ENVIRONMENT`

We can reference the variable like predefined variables:
```yml
run_unit_tests:
  stage: test
  scrip:
    - echo "Running unit tests for micro service $MICRO_SERVICE_NAME ..."
```

**Use Case:** sometimes, we want to use data that are in file format e.g. database info. We can define a file just like defining a variable in settings. The value is saved to a temporary file.

**Note:** Defining variables in project settings is good for sensitive data. We can also define variables in the config file itself. We can define the variable in job level, so only that job has access to it. We can define it on the top global level.

```yml
build_image:
  variables:
    image_repository: docker.io/my-docker-id/myapp
    image_tag: v1.0
  only:
    - main
  stage: build
  script:
    - echo "tagging the docker image $image_repository:$image_tag"
```

## GitLab Architecture

- Server/Instance
- Runner
- Executor

There is a **GitLab server** that is the main component of the architecture and manages the pipeline execution. The jobs are run on **GitLab runners**. GitLab can be configured as a self-hosted service or can be used as a `SaaS` on [it's website](https://www.gitlab.com).

GitLab runners are programs that we should install on a machine, that's separate from the one that hosts the GitLab instance. Runners are shared for all projects in GitLab.

### Executors

GitLab runners do no execute code themselves. They fetch jobs from GitLab and pass the commands to the `shell`. Shell is the simplest **executor**. The executor determines the environment each job runs in. 

**Note:** All required programs for executing the jobs, need to be installed manually. It makes managing dependencies & migration hard.

A better alternative executor to shell, is **Docker Executor**. It executes jobs inside a docker container, and for each job a new fresh container is used. Images are user provided.

Another option is **Virtual Machine Executor**. It has the overhead of OS, and is good for environments that have not docker. 

**Kubernetes Executor** allows us to use an existing kubernetes cluster for our jobs. It will create a new pod for each job, and is highly scalable.

**Docker Machine Executor** supports auto scaling. It let's us create docker hosts on demand, dynamically. It's a managing layer over docker. GitLab shared runners use this executor.
**Note:** Docker Machine is deprecated by docker.

The best option is docker executor for most use cases. We can configure one executor per runner. If we need more executors we can register more runners on the same server.