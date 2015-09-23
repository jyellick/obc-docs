# Getting Started
Welcome to Openchain development!

You are strongly recommended to read the [project definition](techspec.md) to
get an overview of what we are developing. We use agile methodology with weekly
sprint, organized by [issues](https://github.com/issues) tags, so take a look
at the issues to familiarize yourself with the open workitems.

## License
This software is under [Apache License Version 2.0](LICENSE)

## Setting up the development environment
We create a development environment for this project such that every developer
would have the same set up and can be productive within a few minutes. Follow
the [instruction](devenv.md) to download and start.

## Contribution
We are using [Github Flow](https://guides.github.com/introduction/flow/) process
to contribute code.
####Here is the basic Github Flow:
- Anything in the master branch is deployable
- To work on something new, create a descriptively named branch off of master
(ie: new-oauth2-scopes)
- Commit to that branch locally and regularly push your work to the same named
branch on the server
- When you need feedback or help, or you think the branch is ready for merging,
open a pull request
- After your pull request has reviewed and signed off, a committer
can merge it into master branch

You must agree to [Developer's Certificate of Origin](DCO1.1.txt) prior to
submitting code.

## Communication
We will use mainly Slack for communication and Slack Hero for screen sharing
between developers.

There are twice weekly 15-min scrum calls to check in (standard scrum protocol)
and a weekly 1-hour team playback meeting. We will limit the playback to
30 min, and the rest for discussion. You will receive the calendar invite for
these meetings. If you can't attend, kindly let someone know.

## Coding Golang
We require a file [header](headers.txt) on all source code files. Just copy and
paste.
We code in Golang and strictly follow the [best practices](http://golang.org/doc/effective_go.html)
and will not accept any deviation.
