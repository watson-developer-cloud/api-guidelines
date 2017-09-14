***Content-Type Guidlines***

**Honor user-specified content-types when provided**

When the request contains a specific content-type (i.e. one that's not generic like “application/octet-stream”), services should attempt to use this (rather than, say, the filename), although in the event of failure it's reasonable to fallback to other approaches of content-type detection.


**Auto-detect content-type when possible**

In many cases we can detect the content type by introspecting the file contents, and in many cases this is the least ambiguous way of detecting files, for example with image file formats, in which it should be used in lieu of the content-type (which might be overly generic, like “application/octet-stream”) or filename (which might be absent or not have an identifiable extension).

In other cases a combination of filename (when present), content-type (when sufficiently specific) and file content are the best signals for actual content type, for example when accepting textual documents.

In a few cases auto-detection and filenames are insufficient and we need a specific, unambiguous content-type specified by the user, for example with audio input.

Services should follow the most helpful of these approaches that works for their data, to aid in easy exploration of the API; depending on which tools and clients users are using, it may be difficult to generate a filename or content-type, which might be omitted form the request or filled in with generic values.


**Don't require a content-type (or require a non-generic content-type)**

When making a curl request like “curl https://host.com -d file=@./file”, the default may not have a content-type other than the generic “application/octet-stream”; however if we still have enough to process the request we should.


**Don't require filenames when not needed**

A request part might start something like:

content-disposition: form-data;name="negative_examples";filename="filename.ext"
content-type: application/zip

When using SDKs that accept input streams from in-memory objects, there is no useful information to put in the filename, so rather than having the SDKs make up a fake filename like “test.json” or “documents.zip”, services should accept requests without any filename in the request.


