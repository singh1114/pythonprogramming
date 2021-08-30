---
layout: post
title: Automatically Build Python Package using Github actions
date: 2021-08-30T12:51:26.398Z
updated_date: 2021-08-30T12:51:26.442Z
description: How to automatically build and deploy your pypi package whenever
  you are pushing it to some branch.
published: true
image: https://fredysomy.hashnode.dev/_next/image?url=https%3A%2F%2Fcdn.hashnode.com%2Fres%2Fhashnode%2Fimage%2Fupload%2Fv1613359341249%2FFCYSkDPIk.jpeg%3Fw%3D1600%26h%3D840%26fit%3Dcrop%26crop%3Dentropy%26auto%3Dcompress%2Cformat%26format%3Dwebp&w=3840&q=75
tags:
  - python
  - pypi
categories:
  - python
  - pypi
author_name: Fredy Somy
author_username: fredysomy
show_ads: false
show_telegram_signup: false
---
Are you tired of manually building and deploying Python packages to `PyPi`? In this post, we are going to talk about the solution in detail. By the end of the post, you will not have to build your package again.

## Prerequisites - What you need.

*   A github account
*   A PyPi account.
*   A Packagable Python project.

If you dont know how to package a project refer [here](https://packaging.python.org/tutorials/packaging-projects/).

## How - Lets get started

*   Go to GitHub repository containing your python project.
*   Click on the `Actions` tab. 
![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/bnl5b9gpdgxlvj573u3m.png)
*   Select the Publish Python package action.
![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/mbsc64vfk9esjv1zwj7j.png)
*   Notice that the new file which we are committing is in the `.github` directory and inside the workflows directory.

```shell
.github/workflows/python-publish.yml
```
    
*   Let's see the code which is `.yml` file containing some commands to be executed.
    
```yml
    name: Upload Python Package
    
    on:
      release:
        types: [created]
    
    jobs:
      deploy:
    
        runs-on: ubuntu-latest
    
        steps:
        - uses: actions/checkout@v2
        - name: Set up Python
          uses: actions/setup-python@v2
          with:
            python-version: '3.x'
        - name: Install dependencies
          run: |
            python -m pip install --upgrade pip
            pip install setuptools wheel twine
        - name: Build and publish
          env:
            TWINE_USERNAME: ${{ secrets.PYPI_USERNAME }}
            TWINE_PASSWORD: ${{ secrets.PYPI_PASSWORD }}
          run: |
            python setup.py sdist bdist_wheel
            twine upload dist/*
```

*   Let's go over this line by line

```yml
    name: Upload Python Package
    
    on:
      release:
        types: [created]
``` 

*   The `name: " "` is the name of the workflow which you are creating.
    
*   The `"on"` depicts an event which here is the releases event.
    
*   If we want to run the workflow each time we push code .👇

```yml
    on:
      push:
        branches : [ master ]
```

*   Here the event is `"Push"` so each time you push code the workflow runs.

```yml

    jobs:
     .....
```    

*   `jobs` tell the workflow what all jobs have to be done when the workflow is triggered.

```yml
    deploy:
        runs-on: ubuntu-latest
        steps:
        - uses: actions/checkout@v2
        - name: Set up Python
          uses: actions/setup-python@v2
          with:
            python-version: '3.x'
```   

*   `runs-on` specifies which OS it should run on.
*   Then the `steps` is a long list of works to be done.
*   Here the first job is to set up python and the `- name` means the name of the job running.
*   Here is a set of jobs running in my package.

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/85sc8fyq00td3pevj4q3.png)
    
*   Then after setting up python we have to install the dependencies.
    
```yml
    - name: Install dependencies
          run: |
            python -m pip install --upgrade pip
            python -m pip install -r requirements.txt
            pip install setuptools wheel twine
```

*   Here the `run: |` is the set of commands to be executed.
    *   We are Upgrading pip.
    *   Installing the dependencies in requirements.txt
    *   And we are installing setuptools, wheel and twine.
*   setuptools and wheel are for packaging.
*   Twine is for uploading the package to Pypi.
    
*   Here is the final part to Build and Publish a package.
    
```yml
    - name: Build and publish
          env:
            TWINE_USERNAME: ${{ secrets.PYPI_USERNAME }}
            TWINE_PASSWORD: ${{ secrets.PYPI_PASSWORD }}
          run: |
            python setup.py sdist bdist_wheel
            twine upload dist/*
```

*   The `env:` is some environment variables which you need ( Here your PyPi username and password.)
    
    *   You can go to the repo settings and add secrets , which can be accessed by `secrets.envname`
*   In the `"run :"` you are seeing two commands.
    
    *   Here `python setup.py sdist bdist_wheel` builds the package and add it into a dist/ folder.
*   And finally `twine upload dist/*` uploads the distributions to PyPi.
    
*   You can also set up tests if you want.