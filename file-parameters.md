# Best Practices for File Parameters

## Background

APIs that accept file parameters generally must use the `multipart/form-data` content type.

Files sent in `multipart/form-data` may have an associated filename and also an associated media-type.

In Java, this might look like:
```
        RequestBody imagesFileBody = InputStreamRequestBody.create(mediaType, imageStream);
        multipartBuilder.addFormDataPart(<param name>, <file name>, imagesFileBody);
```

The service may require that a filename be supplied along with the file contents.  Two common reasons for requiring the filename is because the service stores the filename as metadata along with the file, or because the service may use the filename -- in particular its extension -- to determine the media type of the file contents. 


The service may require that a media type be supplied along with the file contents.  This typically needed if the file contents may be one of several different media types, and the service uses the supplied media type value to determine how to process the file contents.

When a file parameter may be one of several different media types, there are several mechanisms the service may use to determine the associated media type.  As mentioned above, the service could require the media type to supplied in the request, or could infer the media type from the filename associated with the request.  In addition, it is often possible to determine the media type by inspecting the first few bytes of the file content for a known [file-signature][file-signatures].

[file-signatures]: https://en.wikipedia.org/wiki/List_of_file_signatures 

## Best Practices

### Documentation

- If the service requires that the filename be supplied along with the file contents, this should be specified in the API document.

- If the service requires that the media type be supplied along with the file contents, this should be specified in the API document.

### Determining media type

- Where possible, the service should use file signatures to determine media type, rather than require a media type or infer it from the filename.

- When file signatures cannot be used to determine media type, the service should use the media type passed with the parameter, if present.

- The service should only attempt to infer the media type from the filename when no other means to determine the media type are available.

- If the only use of the filename is to infer the media type, the filename should be required only if no other means to determine the
media type are available.  In particular, the request should not fail if the media type to passed explicitly in the request.

