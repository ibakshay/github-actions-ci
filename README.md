August 17,2019, [IB Akshay](https://github.com/ibakshay) 

`GitHub Actions` is a feature introduced last year and there are already lot of  awesome actions like [automatic-rebase](https://github.com/marketplace/actions/automatic-rebase), [post-slack-message](https://github.com/marketplace/actions/post-slack-message) availabe in the [GitHub MarketPlace](https://github.com/marketplace?type=actions). GitHub  Actions are just automated scripts - reusable piece of code -  totally managed and run by GitHub -  orchestating any workflow  and  can be triggered  based on any [GitHub event](https://developer.github.com/actions/managing-workflows/workflow-configuration-options/#events-supported-in-workflow-files).

Last week, GitHub has introduced beta of GitHub Actions Version 2 with fully integrated Continuous Integration and Delivery (CI/CD) support. I was very excited to try this new feature and I have implemented the GitHub Actions CI for the forked open-source project [CLA Assistant](https://github.com/ibakshay/cla-assistant).  In this post, I will show you how easy It is to get started with Github Actions for CI.

## Why GitHub Actions CI/CD ?

As there are already lot of amazing CI/CD platforms around like Travis, CircleCI, you might be asking why should I choose GitHub Actions over other CI/CD platforms. 

I'm not saying that GitHub Actions is the best CI/CD platform , I'd rather say it totally depends upon your requirements. However, I will just share a few advantages you will get If you are using GitHub Actions for CI/CD.

- Fully integrated with GitHub 
- Can respond to any GitHub Event
- no need to set up any webhook
- no need to login to the CI/CD platform and enable the repository 
- Real-Time logs can be viewed directly on GitHub with Color coding and Emoji support as well 😉

## Setup

Let's get started.. I will walk you through each and every step of the setup process. 
First and foremost, you should have beta access in order to use the GitHub Actions.  If you don't have beta access then you can sign up for the beta [here](https://github.com/features/actions). 

When you navitage to **Actions** tab in your repository, the GitHub will itself recommend the appropriate  Workflows for the project to help you get started. Since the project is written in Javascript, the GitHub is suggesting to setup CI for the node.js App and deploy in GitHub package Registry. 
<img width="1398" alt="Screen Shot 2019-08-18 at 19 40 58" src="https://user-images.githubusercontent.com/33329946/63228334-7d00e300-c1f1-11e9-9f70-db26e7c99675.png">

You can either choose one of the recommended workflows or you can also setup a workflow yourself. 
All the GitHub Actions workflows uses a very neat `YAML` syntax and  should be always stored  in `.github/workflows`of the project root.

Let's start building the workflow to setup the Continuous Integration(CI) for a Node.js App. You can however setup GitHub Actions CI for any language and is not language specific. 

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

One of the feature of GitHub Actions CI which I personally like the most is the Matrix build. The matrix build will allow us to test in multiple versions of the node.js App and we can build not only on `Linux` but   accross `macOS`, `Windows` and `Linux` in parallel and that's really very cool 😎. 

```yaml 
jobs:
  build:
    runs-on: $a{{matrix.os}}
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
#### jobs


