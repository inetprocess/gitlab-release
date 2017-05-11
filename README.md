gitlab-release
====
Tool to create gitlab revisions with file upload.Useful for CI publish tasks.

Usage
----
This program is intended to be used in a GitLab CI job in a Runner with Docker.

### 1. Configure your `.gitlab-ci.yml`
To make an automatic release you need to add something like this to the file `.gitlab-ci.yml` in your project.

```yaml
stages:
    - build
    - publish
build:
    stage: build
    script:
        - my_build_command
    artifacts:
        expire_in: '1 hour'
        paths:
            - compiled-$CI_BUILD_TAG.exe
            - doc-$CI_BUILD_TAG.pdf
publish:
    image: inetprocess/gitlab-release
    stage: publish
    only:
        - tags
    script:
        - gitlab-release --message 'My release message' compiled-$CI_BUILD_TAG.exe doc-$CI_BUILD_TAG.pdf
```

### 2. Generate a personnal access token
Generate a new [Personal Access Token](https://docs.gitlab.com/ee/api/README.html#personal-access-tokens)
from your user profile with the api scope.

### 3. Configure your project
Set a [secret variable](https://docs.gitlab.com/ce/ci/variables/#secret-variables) in your project
named `GITLAB_ACCESS_TOKEN` with the token you have generated in the previous step.

