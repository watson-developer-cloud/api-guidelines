# SDK Guidelines

## Table of Contents

<!--
  The TOC below is generated using the `markdown-toc` node package.

      https://github.com/jonschlinkert/markdown-toc

  You should regenerate the TOC after making changes to this file.

      ./node_modules/.bin/markdown-toc -i sdk-guidelines.md
  -->

<!-- toc -->

- [Introduction](#introduction)
- [General](#general)
  * [Provide SDKs for languages commonly used by developers](#provide-sdks-for-languages-commonly-used-by-developers)
  * [Employ best practices and idioms for each language](#employ-best-practices-and-idioms-for-each-language)
  * [Establish clear coding style guidelines](#establish-clear-coding-style-guidelines)
  * [Minimize burden of authentication](#minimize-burden-of-authentication)
  * [Use standard package management](#use-standard-package-management)
  * [Semantic Versioning](#semantic-versioning)
- [Documentation](#documentation)
  * [Provide clear and complete documentation for each SDK](#provide-clear-and-complete-documentation-for-each-sdk)
  * [SDK Documentation should be discoverable and presented in a form appropriate for each SDK language](#sdk-documentation-should-be-discoverable-and-presented-in-a-form-appropriate-for-each-sdk-language)
  * [SDK documentation should include examples of common flows](#sdk-documentation-should-include-examples-of-common-flows)
  * [Service documentation should reference and incorporate SDKs](#service-documentation-should-reference-and-incorporate-sdks)
  * [Coding examples should use SDKs](#coding-examples-should-use-sdks)
- [Support](#support)
  * [SDKs should be open-source](#sdks-should-be-open-source)
  * [SDKs shoud encourage community contributions](#sdks-shoud-encourage-community-contributions)
  * [SDKs should have well-defined and actively monitored support channels](#sdks-should-have-well-defined-and-actively-monitored-support-channels)
  * [SDKs should be instrumented to allow collection of usage information](#sdks-should-be-instrumented-to-allow-collection-of-usage-information)

<!-- tocstop -->

<!-- --------------------------------------------------------------- -->

## Introduction

The following are guidelines for Software Development Kits (SDKs) for IBM Cloud Services.

These guidelines complement the API Guidelines contained in the API Handbook

- [API Implementation Handbook](https://pages.github.ibm.com/CloudEngineering/api_handbook/)

In addition, Watson APIs should adhere to the Watson Developer Cloud REST API guidelines:

- [WDC REST API guidelines](https://github.com/watson-developer-cloud/api-guidelines)

## General

### Provide SDKs for languages commonly used by developers

Every IBM Service that offers public APIs should offer SDKs for accessing these APIs in the languages most commonly used by developers that use the service.
- Java, Node, and Python are extremely common "first-tier" languages that should have SDK support
- iOS and Android SDKs are important for services that may be used from mobile devices

### Employ best practices and idioms for each language

SDKs should employ the generally accepted best practices and common idioms of the specific language.
They should feel "natural" to developers with experience in that language.
In particular, the JSON structure of the underlying API request and response structures should be converted
into native language objects with appropriately typed member variables.

### Establish clear coding style guidelines

SDKs should have well documented coding style guidelines and wherever possible should employ code style checkers to enforce coding style.

### Minimize burden of authentication

SDKs should minimize the developer burden for common tasks such as authentication, and expose service versioning in a highly consumable fashion.
In particular, these aspects are often best centralized as methods on a _service object_, rather than being exposed on each method of the service.

### Use standard package management

SDKs should be distributed using the most common/popular package management systems for each language, E.g.
- Maven Central for Java SDKs
- npm for Node SDKs
- PyPI for Python SDKs, pip install
- CocoaPods or Carthage for Swift SDKs

### Semantic Versioning

SDKs should use semantic versioning for SDK releases (this requirement is enforced by some package management systems).

## Documentation

### Provide clear and complete documentation for each SDK

SDKs should have accompanying documentation that clearly describes all public methods and classes of the SDK.
This includes full documentation for method parameters and responses and all member variables of classes.
This documentation should -- to the greatest extent possible -- use terminology that is appropriate for the specific language
(e.g. Java objects should not be described as "JSON", and methods for service invocations should be describe as a "GET /v3/path/to/the/service").

### SDK Documentation should be discoverable and presented in a form appropriate for each SDK language

SDK documentation should be delivered in a format that experienced developers for a particular language will find familiar
- Javadoc for Java SDKs
- ? for Node SDKs
- readthedocs.io for Python SDKs
- Jazzy for Swift SDKs

### SDK documentation should include examples of common flows

The SDKs should include examples that illustrate common usage flows involving multiple service APIs.

### Service documentation should reference and incorporate SDKs

Documentation for the service should provide prominent references to the SDKs.
This should include how the SDKs can be accessed through the package management systems appropriate for each language,
and how this can be automated in a DevOps pipeline flow for an application using the SDKs.

### Coding examples should use SDKs

All IBM-developed coding examples for the service should use the SDKs.

## Support

### SDKs should be open-source

SDKs should generally be open-source -- hosted on an IBM public GitHub repository.
- Should be discoverable through simple GitHub and Google searches

### SDKs shoud encourage community contributions

SDKs should encourage community contributions both in words and in actions (by accepting PRs from community developers).
The process for community contributions should be clearly described in the CONTRIBUTING doc of the repo.
The process should require contributors to sign and submit a Contributors Licence Agreement (CLA) before contributions are accepted.

### SDKs should have well-defined and actively monitored support channels

All SDKs should clearly document the proper channels for reporting errors or requesting assistance in using the SDK.
These channels should include GitHub issues, posts to StackOverflow, and other popular developer sites.
These channels should be continuously monitored by the SDK team with responses provided in a timely manner.

### SDKs should be instrumented to allow collection of usage information

SDKs should be instrumented so that relevant usage data can be collected by the service.

