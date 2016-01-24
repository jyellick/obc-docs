## Your voice here

If you are reading this, then it means anything said in this open source repository is open for your input. Apply your experience, lend your voice, and show with your code how the specification and the implementation of Open Blockchain should be taken forward. Don’t like something you see here? Help us all see your point of view (in a respectful and positive way) or even better, bring some code that shows a better way.


## Getting Started
Welcome to Open Blockchain (OBC) development!

If you are new to OBC and want to learn about our position and the scope of the project, please start with our [whitepaper](whitepaper.md). In addition, we encourage you to go over our [glossary](glossary.md) to understand the terms we use throughout the site.

When you are ready to start using OBC to build your applications or wish to contribute to the project, we strongly recommend you to read the [protocol specifications](protocol-spec.md) to get an overview of what we are developing. We use agile methodology with a weekly sprint, organized by [issues](https://github.com/openblockchain/obc-peer/issues) and [milestones](https://github.com/openblockchain/obc-peer/milestones), so take a look to familiarize yourself with the current work.

## Documentation
To find documentation on a specific topic please browse the documentation directories within this repository:
- [Openchain FAQs](https://github.com/openblockchain/obc-docs/tree/master/FAQ)
- [Canonical Use Cases](https://github.com/openblockchain/obc-docs/blob/master/biz/usecases.md)
- [Development Environment Setup](https://github.com/openblockchain/obc-docs/blob/master/dev-setup/devenv.md)
- [Chaincode Development Environment](https://github.com/openblockchain/obc-docs/blob/master/api/SandboxSetup.md)
- [Openchain APIs](https://github.com/openblockchain/obc-docs/blob/master/api/Openchain%20API.md)
- [Openchain Network Setup](https://github.com/openblockchain/obc-docs/blob/master/dev-setup/DevnetSetup.md)
- [Technical Implementation Details](https://github.com/openblockchain/obc-docs/tree/master/tech)

## License
This software is under [Apache License Version 2.0](LICENSE)

## Setting up the development environment
We create a development environment for this project such that every contributor
would have the same set up and can be productive within a few minutes. Follow
the [instructions](dev-setup/devenv.md) to download and start.

## Contribution
We are using [Github Flow](https://guides.github.com/introduction/flow/) process
to contribute code.
####Here is the basic Github Flow:
- Anything in the master branch is deployable
- To work on something new, create a descriptively named branch off of your fork. [More detail on fork](https://help.github.com/articles/syncing-a-fork/)
- Commit to that branch locally and regularly push your work to the same named
branch on the server
- When you need feedback or help, or you think the branch is ready for merging,
open a pull request (make sure successfully built and tested with the test-suite)
- After your pull request has reviewed and signed off, a committer
can merge it into the master branch

We use the same approach - the [Developer's Certificate of Origin](https://github.com/openblockchain/obc-docs/blob/master/biz/DCO1.1.txt) - as the Linux Kernel [community](http://elinux.org/Developer_Certificate_Of_Origin) does to manage contributions.
We simply ask that when submitting a pull request, that the developer include a sign-off statement in the pull request's description.

Here is an example Signed-off-by line, that indicates that the submitter accepts the DCO:

```
Signed-off-by: John Doe <john.doe@hisdomain.com>
```

## Communication
We will use mainly our [Slack for communication](https://openchain.slack.com) and
Screenhero or Google Hangouts for screen sharing between developers. Register and get connected.

## Coding Golang
- We require a file [header](https://github.com/openblockchain/obc-docs/blob/master/dev-setup/headers.txt) on all source code files. Just copy and
paste.
- We code in Go and strictly follow the [best practices](http://golang.org/doc/effective_go.html)
and will not accept any deviation. You must run the following tools against your code and fix all errors and warnings.
	- [golint](https://github.com/golang/lint)
	- [go vet](https://golang.org/cmd/vet/)
