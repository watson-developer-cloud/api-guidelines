# Guidelines for SDK Compatibility

## Table of Contents

<!--
  The TOC below is generated using the `markdown-toc` node package.

      https://github.com/jonschlinkert/markdown-toc

  You should regenerate the TOC after making changes to this file.

      ./node_modules/.bin/markdown-toc -i sdk-compatibility.md
  -->

<!-- toc -->

- [Introduction](#introduction)
  * [Version-dates](#version-dates)
- [Compatible changes](#compatible-changes)
  * [Adding a new operation to the service](#adding-a-new-operation-to-the-service)
  * [Adding a new optional parameter to the end of the parameter list for an operation](#adding-a-new-optional-parameter-to-the-end-of-the-parameter-list-for-an-operation)
  * [Adding a new optional property to a model after previously existing properties](#adding-a-new-optional-property-to-a-model-after-previously-existing-properties)
  * [Changing the description of an operation, parameter, model, or property](#changing-the-description-of-an-operation-parameter-model-or-property)
  * [Adding a new value to an enum.](#adding-a-new-value-to-an-enum)
  * [Adding, removing, or changing a "documentation-only" attribute of a parameter or property](#adding-removing-or-changing-a-documentation-only-attribute-of-a-parameter-or-property)
- [Possibly compatible changes](#possibly-compatible-changes)
  * [Adding a required property to a model that is only used as a response model](#adding-a-required-property-to-a-model-that-is-only-used-as-a-response-model)
  * [Reordering the properties of a model that is not used as a request body](#reordering-the-properties-of-a-model-that-is-not-used-as-a-request-body)
- [Incompatible changes](#incompatible-changes)
  * [Removal of a operation](#removal-of-a-operation)
  * [Removal of a parameter from an operation](#removal-of-a-parameter-from-an-operation)
  * [Changing the operationId of an operation](#changing-the-operationid-of-an-operation)
  * [Reordering of the parameters of an operation](#reordering-of-the-parameters-of-an-operation)
  * [Adding a required parameter to an operation](#adding-a-required-parameter-to-an-operation)
  * [Adding a parameter to an operation anywhere except after the last existing parameter](#adding-a-parameter-to-an-operation-anywhere-except-after-the-last-existing-parameter)
  * [Changing the name of a model](#changing-the-name-of-a-model)
  * [Removal of a model](#removal-of-a-model)
  * [Replacing an "inner model" with a ref to a new top-level model](#replacing-an-inner-model-with-a-ref-to-a-new-top-level-model)
  * [Removal of any property of a model](#removal-of-any-property-of-a-model)
  * [Changing a property from required to optional or vice-versa](#changing-a-property-from-required-to-optional-or-vice-versa)
  * [Removing an enum value](#removing-an-enum-value)

<!-- tocstop -->

<!-- --------------------------------------------------------------- -->

## Introduction

This document describes how changes to an OpenAPI (nee Swagger) API document will affect the
compatibility of SDKs generated from this document.
The incompatible changes listed below should be avoided whenever possible to maintain SDK compatibility.

For the purpose of this document, we consider the newly generated SDK to be incompatible if a program previously developed using the previous version of the SDK will either not compile or function incorrectly if built with the newly generated version of the SDK.

All SDKs adhere to the [Semantic Versioning](https://semver.org/) standard,
which only allows backwards-incompatible changes in a new major version of the SDK.

### Version-dates

Services with a version-date parameter can introduce "minor breaking changes" into a service with a new version-date value.
Nevertheless, the swagger document should, whenever possible, faithfully describe the API for _all_ possible values of version-date,
not just the most recent.
This may mean that certain aspects of service behavior cannot be expressed formally in the swagger,
and must instead be conveyed informally in description fields of operations, parameters, models, or properties.
This is a tradeoff between completeness and simplicity of the interface description:
we should strive for completeness in most cases but exceptions may be made if circumstances warrant.

<!-- ------------------------ -->

## Compatible changes

The following are changes that can be made to the swagger that result in fully compatible SDKs.

### Adding a new operation to the service

New operations are compatible and can be introduced without a version-date change.

### Adding a new optional parameter to the end of the parameter list for an operation

New optional parameters are compatible from an API perspective so long as the default behavior of the service
matches the behavior prior to the change.
A version-date change is not required in this case.

### Adding a new optional property to a model after previously existing properties

Further, a property _can_ be added elsewhere in a model if the model is not used as a request body.
New properties must be added to the end of request bodies because some languages "explode" the request body,
meaning that the properties are converted to parameters on the associated methods.

### Changing the description of an operation, parameter, model, or property

Descriptions in the API document become comments in the SDKs.

### Adding a new value to an enum.

The SDKs intentionally avoid using the enum values for validation so as to allow new enum values to be added.

### Adding, removing, or changing a "documentation-only" attribute of a parameter or property

The SDKs consider the following parameter/property attributes as "documentation only":

- default
- maximum
- exclusiveMaximum
- minimum
- exclusiveMinimum
- maxLength
- minLength
- pattern
- maxItems
- minItems
- uniqueItems
- multipleOf

<!-- ------------------------ -->

## Possibly compatible changes

Need to think through these

### Adding a required property to a model that is only used as a response model

A new required property will result in a new member variable of the corresponding class
in the SDK, and additional data in the objects of that class.

This change is probably compatible so long as the service always includes the new property
in its responses _regardless_ of the version-date specified on the request.

### Reordering the properties of a model that is not used as a request body

Reordering of properties in a request model is likely incompatible because it will change
the order of the parameters in the constructor for the class or potentially the associated
operation, since the generator may convert the model properties into parameters for some SDKs.

But if the model is not used as a request body, then it may be allowable to reorder the parameters.

<!-- ------------------------ -->

## Incompatible changes

The following changes will result in an incompatible change to the SDK and thus should be avoided.
Some items provide advice for alternative approaches that will maintain SDK compatibility.

**General exceptions**: the rules below apply only to operations that have not been excluded from the SDKs
using the `x-sdk-exclude` annotation and models that have not been pruned from the SDKs.

### Removal of a operation

As a general rule, operations should be formally deprecated for some period of time before being removed.
Within this deprecation period, new major versions of the SDK can eliminate the operation
since incompatible changes are permitted in a major version SDK release.

To maintain SDK compatibility, leave the operation in place but have the service return an error return code.

OpenAPI v2 allows an operation to be flagged as deprecated with the boolean `deprecated` attribute.

### Removal of a parameter from an operation

To maintain compatibility, the parameter should be retained but marked as deprecated.
The description should be updated to document that its value is ignored.
Within this deprecation period, new major versions of the SDK can eliminate the parameter.

### Changing the operationId of an operation

The `operationId` is used as the name of the SDK method (adapted to the language naming conventions)
for the operation.
Changing the `operationId` for an operation will likely change the method name in the SDK,
which will break any applications that use the previous method name.

### Reordering of the parameters of an operation

Changing the order of parameters to an operation causes a breaking change in the Python and Swift SDKs.

The Python and Swift SDKs have service methods that accept all parameters in the same order as they appear in the API document.

Changing the order of parameters in the API doc will alter the signatures of these methods, which will be a breaking change.

The exception to this rule is that required parameters can be moved before optional parameters, because the SDK
generator already makes this modification because it is required by the Python and Swift languages.

### Adding a required parameter to an operation

This will break existing applications because they don't provide a value for this parameter.

### Adding a parameter to an operation anywhere except after the last existing parameter

The Python and Swift SDKs have service methods that accept all parameters in the same order as they appear in the API document.

Adding a new parameter into the middle of the parameter list, even if it is optional, which change
the method signature in a way that could break existing invocations of the method.

### Changing the name of a model

The model name is the basis for the corresponding class name in the SDKs.

This can be made compatible by adding an `x-alternate-name` annotation to retain the previous model name in all affected SDKs.

### Removal of a model

Removing a model will remove the corresponding class from the newly generated SDK.
Since this class was present in prior versions of the SDK, applications could contain references
to that class, which will cause build failures when compiled with the new SDK.

To maintain SDK compatibility, leave the model in place but mark it as deprecated.
OpenAPI v3 allows a model(schema) to be marked as deprecated with the boolean `deprecated` attribute.

### Replacing an "inner model" with a ref to a new top-level model

The SDK generator for some languages creates classes for "inner models",
so this change will be incompatible _unless_ the new top-level
model has a name that is mapped to the previously generated classes.

This change can also be made compatible by adding an `x-alternate-name` annotation to retain the previous model name in all affected SDKs.

Example: The `QueryResultMetadata` model in Discovery.

### Removal of any property of a model

To maintain compatibility, the swagger should retain the property but mark it as deprecated.

If the property is an optional property of a response model, the service can drop the property from its responses
but it must remain defined in the API document.

### Changing a property from required to optional or vice-versa

Some languages, e.g. Swift, have language support for defining _optional_ types -- meaning the value may or may not be present.
Optional types are an elegant way to distinguish required from optional properties in response models --
optional properties are declared with optional types whereas required properties are not.

Unfortunately, this means that once defined in the API spec, a property cannot change from required to optional or vice-versa without
resulting in an incompatible change to the generated SDK.

### Removing an enum value

Some languages generate constants for enum values.  If an enum value is removed, any application
that references the constant for that enum value will fail to compile after updating to the new SDK.
