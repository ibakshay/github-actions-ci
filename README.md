August 17,2019, [IB Akshay](https://github.com/ibakshay) 

`GitHub Actions` is a feature introduced last year and there are already lot of  awesome actions like [automatic-rebase](https://github.com/marketplace/actions/automatic-rebase), [post-slack-message](https://github.com/marketplace/actions/post-slack-message) availabe in the [GitHub MarketPlace](https://github.com/marketplace?type=actions). GitHub  Actions are just automated scripts - reusable piece of code -  totally managed and run by GitHub -  orchestating any workflow  and  can be triggered  based on any [GitHub event](https://developer.github.com/actions/managing-workflows/workflow-configuration-options/#events-supported-in-workflow-files). 

Last week, GitHub has introduced beta of GitHub Actions Version 2 with fully integrated Continuous Integration and Delivery (CI/CD) support. I was very excited to try this new feature and I have implemented the GitHub Actions CI for the forked open-source project [CLA Assistant](https://github.com/ibakshay/cla-assistant).  In this post, I will show you how easy It is to get started with Github Actions for CI/CD.

## Why GitHub Actions CI/CD ?

As there are already lot of amazing CI/CD platforms around like Travis, CircleCI, you might be asking why should I choose GitHub Actions over other CI/CD platforms. 

I'm not saying that GitHub Actions is the best CI/CD platform , I'd rather say it totally depends upon your requirements. However, I will just share a few advantages you will get If you are using GitHub Actions for CI/CD.

- Fully integrated with GitHub
- Can respond to any GitHub Event
- no need to set up a  webhook
- no need to login to the CI/CD platform and enable the repository 
- Real-Time logs can be viewed directly on GitHub with Color coding and Emoji support as well 😉

## Setup

Let's get started.. I will walk you through each and every step of the setup process. 
First and foremost, you should have beta access in order to use the GitHub Actions.  If you don't have beta access then you can sign up for the beta [here](https://github.com/features/actions). 

When you navitage to **Actions** tab in your repository, the GitHub will itself recommend the appropriate  Workflows for the project to help you get started. Since the project is written in Javascript, the GitHub is suggesting to setup CI for the node.js App and deploy in GitHub package Registry. 

<img width="1398" alt="Screen Shot 2019-08-18 at 19 40 58" src="https://user-images.githubusercontent.com/33329946/63228334-7d00e300-c1f1-11e9-9f70-db26e7c99675.png">

You can either choose one of the recommended workflows or you can also setup a workflow yourself. 
All the GitHub Actions workflows uses a very neat `YAML` syntax and  should be always stored  in `.github/workflows`of the project root.

Let's start building the workflow together to setup the Continuous Integration(CI) for [CLA Assistant](https://github.com/ibakshay/cla-assistant) project  which is a Node.js Application  😊. You can however setup GitHub Actions CI/CD for any language and is not language specific. 

 ```yaml 
name: Node CI
on:
  push:
    branches: 
    - master
    - release
    - release-green
  pull_request:
    branches:
    - master
    - release
    - release-green
```

#### name

In the first line of the workflow file, we can name the workflow  as `Node CI`. The GitHub displays this name for the workflow in the repository's  **Actions** page. If you don't give this name then the GitHub sets the name to workflow filename. 

```yaml 
name: Node CI
```
#### on 

Now, we have to specify on what [GitHub event](https://developer.github.com/actions/managing-workflows/workflow-configuration-options/#events-supported-in-workflow-files), this workflow should be triggered. We can also choose array of  [GitHub events](https://developer.github.com/actions/managing-workflows/workflow-configuration-options/#events-supported-in-workflow-files) for the workflow to be triggered. 
 Let's setup the CI for any **push** or **pull_request** event for these specific branches **master**, **release**, **release-green**.
 
  ```yaml 
on:
  push:
    branches: 
    - master
    - release
    - release-green
  pull_request:
    branches:
    - master
    - release
    - release-green
```
 

## Setting up Matrix build

One of the coolest feature of GitHub Actions CI/CD is the Matrix build. The matrix build will allow us to test in multiple versions of the node.js App and we can build not only on `Linux` but accross `macOS`, `Windows` and `Linux` in parallel and that's really very cool 😎. 

### jobs

A workflow is made up of one or more jobs and each job will have a unique Id. All the jobs are run in parallel by default, However, we can also make the jobs run in sequence. Like, we can make one job wait for the other and so on and so forth. Each job runs in a fresh instance of the virtual environment specified by `runs-on`. For our case, We have only one job. 

```yaml 
jobs:
  build:
    runs-on: $ ((matrix.os))
    strategy:
      matrix: 
        node_version: 
        - 8
        - 10
        - 12
        
        os: 
        - ubuntu-latest
        - windows-latest
        - macOS-latest
```


### steps
Every job consists of  a sequence of tasks called **steps**. A step can run  an action from  our repository, any public repository or an action published in a docker registry.  A step can also run commands like `npm install` `npm run test` and can be a setup task. Each step has it's own ID and has access to the workspace and the file system.  

```yaml 
 steps:
    - uses: actions/checkout@master
    - name: Use Node Environment
      uses: actions/setup-node@v1
      with:
        version:  $(( matrix.node_version ))
    - name: Bower
      run: bower install
    - name: Install
      run: npm install
    - name: grunt build and test
      run: |
        grunt build
        grunt test
        grunt coverage
    - name: Test Coverage
      uses: coverallsapp/github-action@master
      with:
        github-token: $(( secrets.github_token ))
        path-to-lcov: ./output/coverage/lcov.info
```
Let's break this down to small pieces and discuss in detail. 

#### Setting up the Node Environment
The first thing we have to do is to select an `action` for setting up the node environment. An `action` is just a reusable piece of code. The GitHub has already provided  [actions/setup-node](https://github.com/actions/setup-node) action  and so, we can re-use this action for setting up our node environment. You can view all the `actions` provided by GitHub [here](https://github.com/actions). Always when we are using an `action` we should go through the README file, because some actions require inputs like environmental variables from us which we need to  set using **with** keyword. So it's always a good idea to review the action's README file to determine the inputs required. 

```yaml
   - uses: actions/checkout@master
    - name: Use Node.js 12.6
      uses: actions/setup-node@v1
      with:
        version:  $(( matrix.node_version ))
```
#### Build Configuration 
After adding the action to setup our node environment, we have to specify the build options. In our case, I have used `bower` and `NPM` as the package managers and `grunt` as a task runner for building, testing and code coverage of the project. You can use any task runners like `gulp`, `webpack` instead of `grunt` depending upon your project configuration. 

```yaml
    - name: Bower
      run: bower install
    - name: Install
      run: npm install
    - name: grunt build and test
      run: |
        grunt build
        grunt test
        grunt coverage
```
#### Test Coverage

Finally I have used the action from [Coverralls](https://github.com/coverallsapp/github-action/) for tracking the code coverage. And yeah, If you are using any other code coverage tool like `codecov`, then you can use their action instead.

```yaml
    - name: Test Coverage
      uses: coverallsapp/github-action@master
      with:
        github-token: $(( secrets.github_token ))
        path-to-lcov: ./output/coverage/lcov.info
```

## *That's it, We are  Done 🎉*
 As you can see, we can setup this configuration in less than 10 minutes and  we don't need to set up any webhook or toggle to enable the repository in the CI provider website for building our repo. Everything is integrated with GitHub. 

#### Real-time Logs

The logs are real-time and GitHub streams our logs to the Actions console to show the logs status progressively. The Log console is simple and neat. We can deep-link to any line in any log file with permalink and most importantly Emoji Support is there as well 😉

![Screen Shot 2019-08-19 at 16 26 16](https://user-images.githubusercontent.com/33329946/63273355-1f76a000-c29e-11e9-9fd3-0431db8fbb0a.png)

#### Deployment

And finally you can deploy to any cloud as you like and there are already actions available in the GitHub marketplace for the deployment which you can directly use it in your workflow file. You can find all the actions for the deployment [here](https://github.com/marketplace?category=deployment&type=actions). 
 
 

