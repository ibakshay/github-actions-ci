August 17,2019, [IB Akshay](https://github.com/ibakshay) 

`GitHub Actions` is a feature introduced last year and there are already lot of  awesome actions like [automatic-rebase](https://github.com/marketplace/actions/automatic-rebase), [post-slack-message](https://github.com/marketplace/actions/post-slack-message) availabe in the [GitHub MarketPlace](https://github.com/marketplace?type=actions). GitHub  Actions are just automated scripts - reusable piece of code -  totally managed and run by GitHub -  orchestating any workflow  and  can be triggered  based on any [GitHub event](https://developer.github.com/actions/managing-workflows/workflow-configuration-options/#events-supported-in-workflow-files).

Last week, GitHub has introduced beta of GitHub Actions Version 2 with fully integrated Continuous Integration and Delivery (CI/CD) support. I was very excited to try this new feature and I have implemented the GitHub Actions CI for the forked open-source project [CLA Assistant](https://github.com/ibakshay/cla-assistant).  In this post, I will you show you how easy It is to get started with Github Actions for CI.

### Why GitHub Actions CI/CD ?

As there are already lot of amazing CI/CD platforms around like Travis, CircleCI, you might be asking why should I choose GitHub Actions over other CI/CD platforms. 

I'm not saying that GitHub Actions is the best CI/CD platform , I'd rather say it totally depends upon your requirements. However, I will just share a few advantages you will get If you are using GitHub Actions for CI/CD.

- Fully integrated with GitHub 
- Can respond to any GitHub Event
- no need to set up any webhook
- no need to login to the CI/CD platform and enable the repository 
- Real-Time logs can be viewed directly on GitHub with Color coding and Emoji support as well 😉

### Setup

Let's get started.. I will walk you through each and every step of the setup process. 
First and foremost, you should have beta access in order to use the GitHub Actions.  If you don't have beta access then you can sign up for the beta [here](https://github.com/features/actions). 

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/ibakshay/github-actions-ci/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
