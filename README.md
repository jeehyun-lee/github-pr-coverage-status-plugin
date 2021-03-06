# GitHub Pull Request Coverage Status Plugin for Jenkins

[![Build Status](https://jenkins.ci.cloudbees.com/job/plugins/job/github-pr-coverage-status-plugin/badge/icon)](https://jenkins.ci.cloudbees.com/job/plugins/job/github-pr-coverage-status-plugin/)
[![Coverage Status](https://coveralls.io/repos/github/terma/github-pr-coverage-status/badge.svg?branch=master)](https://coveralls.io/github/terma/github-pr-coverage-status?branch=master)

* [Overview](#overview)
* [Supports coverage reports](#supports-coverage-reports)
* [How to use](#how-to-use)
* [Master Coverage from Sonar](#master-coverage-from-sonar)
* [Troubleshooting](#troubleshooting)
* [Changelog](#changelog)

## Overview

https://wiki.jenkins-ci.org/display/JENKINS/GitHub+PR+Coverage+Status+Plugin

Parse code coverage report generated by build and post code coverage status comment to GitHub pull request:
![Example](https://raw.githubusercontent.com/jenkinsci/github-pr-coverage-status-plugin/master/screenshot.png)

## Supports coverage reports
* Jacoco ```jacoco.xml```
* Cobertura ```cobertura.xml``` or ```cobertura-coverage.xml```
* Clover ```clover.xml```
* [SimpleCov JSON (Ruby)](https://github.com/vicentllongo/simplecov-json) ```coverage.json```
 
## How to use
* Select or create new Jenkins build
* Ensure that build create code coverage report file
* Configure build to be triggered by [GitHub pull request builder plugin](https://wiki.jenkins-ci.org/display/JENKINS/GitHub+pull+request+builder+plugin)
* Install plugin from Jenkins Plugin Repository
* Or manually 
  * Goto [releases](https://github.com/jenkinsci/github-pr-coverage-status-plugin/releases)
  * Download latest version of plugin *.hpi* file
  * Install to your Jenkins [guide](https://wiki.jenkins-ci.org/display/JENKINS/Plugins)
* Configure global settings of plugin
  * GitHub API URL keep blank for GitHub.com and fill if for dedicated instance of GitHub, example: ```http(s)://hostname/api/v3/```
  * Set [Personal Access Token](https://github.com/blog/1509-personal-api-tokens) (or keep blank if anonymous access enabled)
* Setup Master Coverage (options)
  * Add *Record Master Coverage* post build step to build which test your master
  * Or goto ```Manage Jenkins``` and use ```Use Sonar for master coverage```
* Add *Publish coverage to GitHub* post build step to build. It should be triggered by [GitHub pull request builder plugin](https://wiki.jenkins-ci.org/display/JENKINS/GitHub+pull+request+builder+plugin)

## Master Coverage from Sonar

You have the option to get the master coverage (base coverage) from your SonarQube instance. Otherwise the plugin will keep track of the master coverage in the project configuration file.

* Goto ```Manage Jenkins```
* Find section ```Coverage status for GitHub Pull Requests```
* Fill ```Sonar URL``` field - e.g. `http://sonar.mycompany.com`
* Turn On ```Use Sonar for master coverage```
* The next pullrequest build will use SonarQube as the the source for master coverage

The plugin will try to find the project in SonarQube based on the repository name. If more than one projects match, a waring is logged, and the first one will be used to get the coverage data. 

This feature is still very basic. e.g. authorization against SonarQube is not yet implemented. Feel free to contribute.

## Troubleshooting

### No coverage picture and my Jenkins is in private network and not accessible for GitHub

Because of that GitHub can't render icon picture hosted on Jenkins. Plugin can use http://shields.io public resource instead of Jenkins hosted picture. To use it: 

* Goto ```Jenkins Configuration``` 
* Turn on ```Jenkins is not accessible for GitHub```
* Done next coverage comment in Pull Request will use http://shields.io

### No coverage picture on GitHub Pull Request Page

If you see next picture on your pull request in GitHub:

![No coverage picture](https://raw.githubusercontent.com/jenkinsci/github-pr-coverage-status-plugin/master/no-coverage-picture.png)

In most cases that mean that your Jenkins runs on ```http``` instead of ```https```
Plugin uses same protocol as your Jenkins, however GitHub in most cases accessible by ```https```
Default browser policy to block non secure resources on secure pages from unknown domain.

To fix that:

1. Configure Jenkins to be accessible for ```https``` https://wiki.jenkins-ci.org/display/JENKINS/Starting+and+Accessing+Jenkins
1. Next you have a few options:
  * Run your Jenkins on ```https``` only
  * Or configure plugin to publish link on picture over ```https```
    * Open Jenkins
    * Click ```Manage Jenkins```
    * Click ```Configure System```
      * Find section ```Coverage status for GitHub Pull Requests```
      * Find property ```Jenkins URL for icon in Pull Request```
      * Put URL to your Jenkins with ```https``` like ```https://jenkins.my.com```
      * Save
    * Restart Jenkins

![Plugin settings](https://raw.githubusercontent.com/jenkinsci/github-pr-coverage-status-plugin/master/plugin-settings.png)

## Changelog

### 1.5.0

Supporting Master Coverage from SonarQube. Check (details)[#master-coverage-from-sonar]

### 1.4.0

Supporting of private Jenkins with public GitHub. To enable that mode goto Jenkins Configuration and turn on ```Jenkins is not accessible for GitHub```

### 1.3.0

Add support SimpleCov JSON coverage report for Ruby

### 1.2.0

Add alternative text to coverage image for case when GitHub doesn't able to show image

### 1.1.1

Correct parsing Cobertura report for edge cases 

### 1.0.8

First public release to Jenkins Plugin Repo
