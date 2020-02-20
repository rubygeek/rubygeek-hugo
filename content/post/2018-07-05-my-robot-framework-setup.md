---
categories:
- robot framework
- testing
comments: true
date: "2018-07-05T00:00:00Z"
title: My Robot Framework Setup
---

I've been using [Robot Framework](http://robotframework.org/) for a little over a year now.
I have a handful of commits between Robot Framework (RF) and the RF Selenium Library and I'm active in the Robot Slack Group. It is a really nifty test
automation framework.

This is a little bit about how my tests are arranged and how I setup a new project.

Sample structure:

```
├── README.md
└── suites
    ├── api
    │   ├── api_common.robot
    │   ├── product.robot
    │   └── user.robot
    ├── common.robot
    └── website
        ├── homepage.robot
        ├── login.robot
        ├── search.robot
        └── website_common.robot
```

Each robot file is a test suite and contain the tests. The "common" files contain my settings and keywords.

Now here's what is in the main "common" file:

in my main common file I have the following:

suites/common.robot
```
*** Variables ***
${STACK}  local
${SCREEN}  desktop
${BROWSER}  chrome

${BASE_URL}=   https://www.myawesomepage.com

*** Keywords ***
Open Browser Stack
    [Arguments]    ${URL}
    Open Browser   url=${URL}   browser=${BROWSER}   remote_url=${RemoteUrl}   desired_capabilities=browser:${BROWSER},os:${OS}

Open Test Browser
    [Arguments]    ${URL}
    # default as defined at top of file is 'local'
    Run Keyword If  "${STACK}" == "browserstack"  Open Browser Stack     ${URL}
    Run Keyword If  "${STACK}" == "local"         Open Browser  ${URL}  ${BROWSER}
```

This also makes it easier to run on browser stack by passing along a variable "-v STACK:browserstack". By default it runs "local".

Why wrap the Open Browser keyword in another keyword? I'll show you why in a bit. This is slimmed down for this blog post :)

Notice the variables at the top, they are the "defaults" that are used unless you override them with a command line argument.

Ok so once I have my file structure setup, and my common files setup: Here's my first test.

Why set the url of the page in a variable? This makes it easy to pass in a staging or qa url there if you have multiple sites. If you don't, then you have a easy to do that in future.

suites/website/homepage.robot
```
*** Settings ***
Documentation  My Amazing Homepage test
Resource       website_common.robot
Test Setup     Open Test Browser  ${BASE_URL}
Test Teardown  Close All Browsers

*** Test Cases ***
Ensure Header Appears
   Page Should Contain Element   css=.header
```

Then for  my search page (notice I was able to use the $BASE_URL to specify the url):

suites/website/search.robot
```
*** Variables ***
${SEARCH_URL}=  ${BASE_URL}/search

*** Settings ***
Documentation  My Amazing Search test
Resource       website_common.robot
Test Setup     Open Test Browser  ${SEARCH_URL}
Test Teardown  Close All Browsers

*** Test Cases ***
Ensure Search Appears
   Page Should Contain Element   css=.search-box
```

I said I had more reasons to wrap `Open Browser` .. sometimes you might want to do something on each test like Log the current browser version or resize the window.

```
Set Window Desktop
   Set Window Size   1280  1024

Set Window Tablet
   Set Window Size    768   1024

Set Window Mobile
   Set Window Size    320   568

Set Test Browser Size
    Run Keyword If  "${SCREEN}" == "desktop"  Set Window Desktop
    Run Keyword If  "${SCREEN}" == "tablet"   Set Window Tablet
    Run Keyword If  "${SCREEN}" == "mobile"   Set Window Mobile
    Run Keyword If  "${SCREEN}" == "max"      Maximize Browser Window
    Set Suite Metadata    Browser Size    ${SCREEN}

Log Browser Version
    # the default log level logs the evaluation of this command in the report
    ${browser_version}=   Execute Javascript    return navigator.userAgent
    Set Suite Metadata    Browser Version    ${browser_version}
```

See these keywords I have defined to do those tasks, and if you want them run each time you can put them in your `Open Test Browser` keyword like this:

```
Open Test Browser
    [Arguments]    ${URL}
    # default as defined at top of file is 'local'
    Run Keyword If  "${STACK}" == "browserstack"  Open Browser Stack     ${URL}
    Run Keyword If  "${STACK}" == "local"         Open Browser  ${URL}  ${BROWSER}
    Log Browser Version
    Set Test Browser Size
```

You can run your tests for different size browsers now by passing in the variable for screen, like this `-v SCREEN:mobile`.

Putting the browser in the test report helps when two people run the test and get different results! You could have each person check their version of the browser but its nice to have it in the report.

Thats how I setup my robot tests, let me know if you see anything that I can do better :)
