# Swagger Coding Style Guidelines

## Table of Contents

<!--
  The TOC below is generated using the `markdown-toc` node package.

      https://github.com/jonschlinkert/markdown-toc

  You should regenerate the TOC after making changes to this file.

      ./node_modules/.bin/markdown-toc -i swagger-coding-style.md
  -->

<!-- toc -->

- [Introduction](#introduction)
- [Models](#models)
  * [Model names](#model-names)
  * [Combine models when practical](#combine-models-when-practical)
  * [Descriptions](#descriptions)
  * [Use well-defined property types](#use-well-defined-property-types)
  * [Avoid reserved words](#avoid-reserved-words)
  * [Proper use of required](#proper-use-of-required)
  * [Order of properties](#order-of-properties)
  * [Order of properties in body parameter models](#order-of-properties-in-body-parameter-models)
  * [Sibling elements for refs](#sibling-elements-for-refs)
  * [Use of discriminator field](#use-of-discriminator-field)
- [Operations](#operations)
  * [Order of operations](#order-of-operations)
  * [Summary and description](#summary-and-description)
  * [OperationId](#operationid)
  * [Explicitly specify consumes type(s)](#explicitly-specify-consumes-types)
  * [Do not explicitly define a `content-type` header parameter](#do-not-explicitly-define-a-content-type-header-parameter)
  * [Explicitly specify produces type(s)](#explicitly-specify-produces-types)
  * [Do not explicitly define a `accept-type` header parameter](#do-not-explicitly-define-a-accept-type-header-parameter)
- [Parameters](#parameters)
  * [Use well-defined parameter types](#use-well-defined-parameter-types)
  * [Use refs for common parameters](#use-refs-for-common-parameters)
  * [Specify common parameters for a path in the path definition](#specify-common-parameters-for-a-path-in-the-path-definition)
  * [Parameter order](#parameter-order)
  * [Models for optional body parameters](#models-for-optional-body-parameters)
- [Conventions / Annotations for SDK generation](#-conventions--annotations-for-sdk-generation)
  * [Title and version](#title-and-version)
  * [Error response models](#error-response-models)
  * [Dynamic properties in models](#dynamic-properties-in-models)
  * [Alternate names for properties or parameters](#alternate-names-for-properties-or-parameters)
  * [Excluding operations from the SDKs](#excluding-operations-from-the-sdks)
  * [Array item names](#array-item-names)
  * [Java builders](#java-builders)
  * [File content types](#file-content-types)
  * [Filenames](#filenames)
  * [VCAP_SERVICES](#vcap_services)
  * [Version dates](#version-dates)

<!-- tocstop -->

<!-- --------------------------------------------------------------- -->

## Introduction

The following are guidelines for writing API descriptions using Swagger.
Of course, all Swagger API documents should conform to the Swagger/OpenAPI specification:

- [Swagger specification](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md)
- [OpenAPI specification](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/3.0.0.md)

In addition, Watson APIs should adhere to the Watson Developer Cloud REST API guidelines:

- [WDC REST API guidelines](https://github.com/watson-developer-cloud/api-guidelines)

The guidelines in this document extend and/or clarify the Swagger spec and API guidelines
to address aspects of API specification related to the generation of client libraries
suitable for distribution in a Software Development Kit (SDK).

## Models

### Model names

Model names should be simple, descriptive, and meaningful to developers.
Model names should be in ["upper camel case"](https://en.wikipedia.org/wiki/Camel_case).

Good:

* `Trait`
* `Classifier`

Bad:

* `TraitTreeNode`
* `GetClassifiersTopLevelBrief`

### Combine models when practical

Use a single model to describe similar, compatible objects when practical.
For example, if a "Foo" has 8 properties and a "FooPlus" has the same 8 properties as "Foo" but also two more,
it is usually preferable to combine these two models by adding the two additional properties from "FooPlus" into "Foo"
as optional properties.

### Descriptions

Every model and property should have a description.
These descriptions should match the API Reference descriptions wherever practical.
Avoid describing a model as a "JSON object" since this will be incorrect for some SDKs.
Rather, use the generic "object" in model descriptions.

Good:

* "An object containing request parameters."

Bad:

* "JSON object containing request parameters."

### Use well-defined property types

Model properties and parameters should have well-defined type and format information.
Only use combinations of `type` and `format` defined in the
[Swagger Specification](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md#data-types)

Good:
```
    "matching_results": {
        "type": "integer",
        "format": "int32"
    },
```

Bad:
```
   "matching_results": {
       "type": "number",
       "format": "integer"
    },
```

In the "Bad" example, `matching_results` may appear to be defined as an integer, but the `integer` format is
not defined for type `number`, so the actual type is a floating point (generic number).
The standard tools do not warn about this because the swagger spec does not restrict the values for "type" or "format".

### Avoid reserved words

Avoid using reserved words for model/property names (e.g. `error`, `return`, `type`, `input`).
See [Alternate names for properties or parameters](#alternate-names-for-properties-or-parameters) below
for a way to deal with existing properties whose names are reserved words.

### Proper use of required

Mark a property as "required" if and only if it will be present *and not null* for every instance of the model.

### Order of properties

The properties in a model definition should appear in the same order they should appear in the SDK.
Typically important or fundamental properties should be listed first and ancillary properties appearing last.
For example, if a model has an "id" property that uniquely identifies an instance of the model, that should generally appear earlier in the list of properties.
As a corollary, required properties should generally appear before optional properties in the model definition.

### Order of properties in body parameter models

Models that represent body parameters may be absorbed into the parameter list for the method for the request, so additional care is needed in defining these model:

- List all required properties before any optional properties.
- Add new optional properties to the *end* of the property list.

### Sibling elements for refs

Be aware that the JSON Schema specification (an underlying element of the swagger/OpenAPI spec) does not allow "sibling" elements to a $ref.
(See this [swagger editor github issue](https://github.com/swagger-api/swagger-editor/issues/1184))..
This means that a property defined with a $ref cannot be given an alternate description (or any other attribute).

### Use of discriminator field

The `discriminator` field of a model can be used to create a polymorphic relationship between models. The `discriminator` should be specified on the superclass, although the value doesn't actually affect the relationship. The subclasses should use the `allOf` property to reference the superclass and define any additional properties. 

Example:

```
    "Pet": {
        "type": "object",
        "discriminator": "pet_type",
        "properties": {
            "name": {
                "type": "string"
            },
            "age": {
                "type": "integer",
                "format": "int32"
            }
        }
    },
    "Dog": {
        "allOf": [
            {
                "$ref": "Pet"
            },
            {
                "properties": {
                    "breed": {
                        "type": "string"
                    }
                }
            }
        ]
    },
    "Hamster": {
        "allOf": [
            {
                "$ref": "Pet"
            },
            {
                "properties": {
                    "fur_color": {
                        "type": "string"
                    }
                }
            }
        ]
    }
```

Using `discriminator` allows for the `Pet` class to show up as a `parent` property for both `Dog` and `Hamster` in the SDK generator. Similarly, `allOf` will ensure that the resulting `Dog` and `Hamster` objects contain the properties derived from `Pet`.

<!-- --------------------------------------------------------------- -->

## Operations

### Order of operations

Define basic/common operations before advanced/rare operations.
Typically this order should match the order of operations in the API Reference.

### Summary and description

Each operation should have a summary and/or description.
When both a summary and description are provided, the description should include additional detail
about the operation behavior -- it should not simply restate the summary.

### OperationId

Every operation should have a unique `operationId`.
In the Swagger specification, `operationId`s are optional, but if specified, must be unique.
For SDK generation, `operationId`s are used as the name of the method corresponding to the operation, so it is important to include these in the swagger.

The `operationId` should be specific and descriptive. Here is the recommended convention:

    - GET a single `Model`: `getModel`
    - GET a list of `Model`: `listModels`
    - POST a new `Model`: `createModel` or `addModel`
    - PUT an update to a `Model`: `updateModel`
    - DELETE a `Model`: `deleteModel`

### Explicitly specify consumes type(s)

All POST and PUT operations should explicitly specify their `consumes` type(s).
Note that Swagger (v2) supports a global `consumes` setting to specify the content type(s) consumed
by any API that does not explicitly override this value.

### Do not explicitly define a `content-type` header parameter

Operations that consume multiple content types often use a "content-type" header parameter to specify
the content type of data provided.
However, this header parameter should not be coded explicitly in the swagger, since it is implicitly
specified by a `consumes` setting with more than one value.
The API explorer generates an entry field for content-type based on consumes, so having an explicit
content-type header parameter leads to redundant and confusing means to specify content type in the
API explorer.
The Watson SDK generator ignores any explicitly coded `content-type` header parameter.

### Explicitly specify produces type(s)

All operations should explicitly specify their `produces` type(s).
Note that Swagger (v2) supports a global `produces` setting to specify the content type(s) produced
by any API that does not explicitly override this value.

### Do not explicitly define a `accept-type` header parameter

Operations that produce multiple content types often use an "accept-type" header parameter to specify
the content type of data to be returned.
However, this header parameter should not be coded explicitly in the swagger, since it is implicitly
specified by a `produces` setting with more than one value.
The API explorer generates an entry field for accept-type based on produces, so having an explicit
accept-type header parameter leads to redundant and confusing means to specify accept type in the
API explorer.
The Watson SDK generator ignores any explicitly coded `accept-type` header parameter.

<!-- --------------------------------------------------------------- -->

## Parameters

### Use well-defined parameter types

Parameters should have well-defined type and format information.
Only use combinations of `type` and `format` defined in the
[Swagger Specification](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md#data-types)
Parameter types are further constrained by their "in" property, as specified in
[Swagger Specification, Parameter Object](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md#parameter-object).

Parameters that may have multiple values (e.g. a comma-separated list of values) are best described as arrays of the base value type.
The `collectionFormat` attribute is used to specify how the multiple values are represented, with the default being comma-separated values (CSV).

Good:
```
{
    "name": "return",
    "in": "query",
    "type": "array",
    "items": {
        "type": "string"
    },
    "description": "A comma separated list of the portion of the document hierarchy to return.",
{
```

Bad:
```
{
    "name": "return",
    "in": "query",
    "type": "string",
    "description": "A comma separated list of the portion of the document hierarchy to return.",
{
```

### Use refs for common parameters

For any parameters that appear on multiple operations,
create a named parameter in the parameters section of the swagger doc and
then use a `$ref` to reference the parameter definition from every operation
that accepts this parameter.

### Specify common parameters for a path in the path definition

Any parameter that appears on all operations of a particular path should be specified
in the parameter list for the path rather than in the parameter list for each of
the operations.
This makes the API description more concise and easy to understand.

### Parameter order

List parameters in the order they shall appear in the SDKs:

- List all required parameters before any optional parameters.
  - For services that take a version parameter, list it first since it is always required.
- Add new optional parameters to the *end* of the parameter list.

### Models for optional body parameters

Don't specify required properties in the schema of an optional body parameter.
This is ambiguous and can lead to incorrect implementation on the client or server.

<!-- --------------------------------------------------------------- -->

## <a name="annotations"></a> Conventions / Annotations for SDK generation

### Title and version

The SDK generator assumes that the `title` in the info section is the service name (e.g. "Conversation", and not "Conversation APIs" or some such)
and the `version` truncated at the "." is the "V" version of the service.

### Error response models

The SDK generator assumes that Error responses are defined by a model whose name starts with "Error".
The SDK generator does not generate models for Error responses -- they are handled inline.

The following is an example of a well-designed error response model:
```
    "ErrorModel": {
      "required": [
        "code",
        "error"
      ],
      "properties": {
        "code": {
          "type": "integer",
          "description": "The HTTP status code."
        },
        "error": {
          "type": "string",
          "description": "A message intended for users that describes the error that occurred."
        },
        "help": {
          "type": "string",
          "description": "A URL to documentation explaining the cause and possibly solutions for this error."
        }
      }
    }
```

### Dynamic properties in models

Some models may allow _dynamic properties_ -- properties that are not explicitly named in the schema
(e.g. input, output, and context in Conversation).
This should be explicitly specified with `additionalProperties` in the model definition.

Note that `additionalProperties` is treated differently by the swagger tools from its meaning in JSON schema.
In particular, `additionalProperties` is the default in JSON Schema -- dynamic properties are allowed unless explicitly prohibited.
The swagger tools default the other way, assuming no dynamic properties unless `additionalProperties` is specified
(see discussion [here](https://support.reprezen.com/support/solutions/articles/6000162892-support-for-additionalproperties-in-swagger-2-0-schemas)).

### <a name="annotations"></a>Alternate names for properties or parameters

The `x-alternate-name` annotation can be added to a property or parameter in the swagger to specify an
alternate name for that property or parameter in the SDKs.
The primary use for this annotation is to rename properties/parameters whose original name is
a reserved word in one of the SDK languages.

### Excluding operations from the SDKs

It may be desirable to exclude some operations from the generated SDKs.
For example, Watson Tone Analyzer has GET and POST operations that perform essentially the same function.
SDKs generally only implement the POST operation since it is more flexible and the complexities of coding the POST are hidden by the SDK.

Operations that should not have methods generated in the SDKs should be annotated with `"x-sdk-exclude": true`

### Array item names

Swagger (v2.0) has no provision for giving a name to the elements of an array property.
This is a problem for the SDK generator if it wants to create a method to add or access a single element of the array.

Specify the `"x-item-name"` annotation on the array property with the desired item name.

### Java builders

Models that should have an associated builder object in Java should be annotated with `"x-java-builder": true`.

### File content types

Swagger (v2.0) has no provision for specifying the allowable content-types for files passed in multi-part form bodies.
This information is needed by the generator to determine whether the SDK should support a fixed or user-supplied
content_type for the file.

Use the `"x-file-content-types"` annotation on a file parameter to specify an array of allowable content-types for the
file.  The first value in this array will be used as the default content-type.

### Filenames

Some operations that accept a file parameter require a filename to be supplied along with the file contents.
Typically this is because the service wants to store the filename as metadata associated with the file contents.

To specify that the service requires a filename to be provided along with the file contents, add the `x-include-filename`
annotation to the file parameter with the value `true`.

### VCAP_SERVICES

Applications that run in Bluemix can obtain credentials for thier associated services from the `VCAP_SERVICES` environment
variable.

Specify the `x-vcap-services-name` annotation in the info section of the swagger file with the name of the service
as it appears in the `VCAP_SERVICES` environment variable to enable the SDK to obtain these credentials.

### Version dates

Services that accept version-date parameters may want to define constants in the SDK that developers can use to request
a particular service version.

Specify the `x-version-dates` annotation in the info section of the swagger file with an array of version date values
that should be included as constants in the generated SDK.

