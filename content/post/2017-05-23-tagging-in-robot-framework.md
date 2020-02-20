---
categories:
- testing
- python
- automation
- robot framework
comments: true
date: "2017-05-23T00:00:00Z"
title: Tagging in Robot Framework
---

I talked about the awesome robot framework [in the previous post](/2017/05/21/automation-testing-with-robot-framework/) and I wanted to talk about Tagging since I think this is very useful.

Tagging is one way to group tests so you run a subset of tests. In your test case, add this:

```
Homepage Loads
  [Tags]  smoke
  Open Homepage
  Element Should Be Visible  ${FIND_LOGO}
  [Teardown]  Close Browser
```


Results to run just one tag:

```
▶ robot --include smoke youtube.robot
==============================================================================
Youtube :: A test to demo testing YouTube
==============================================================================
Homepage Loads                                                        | PASS |
------------------------------------------------------------------------------
Search Loads Results                                                  | PASS |
------------------------------------------------------------------------------
Youtube :: A test to demo testing YouTube                             | PASS |
2 critical tests, 2 passed, 0 failed
2 tests total, 2 passed, 0 failed
==============================================================================
```

You can create a `focus` so it is easy to run that one test as you are developing it. I have used a similar technique when working on rspec tests. When the test works as you wanted, remove the focus tag. :)

When you look at the report you can see the tags assigned to teach tests. This report was run on all tests.
![Screenshot](/images/tags-test.png)

