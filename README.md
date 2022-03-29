# openapi-php-lib

A PHP Library to assist in creation of OpenApi Specifications

# Supported OpenApi Version

[Version 3.1.0](https://spec.openapis.org/oas/v3.1.0.html).

# Supported Output Formats

Currently the library supports converting the OpenApi spec to an array which can easily be encoded as JSON in an endpoint or serialized for caching and later delivery as JSON.

[Support for YAML](https://spec.openapis.org/oas/v3.1.0.html#format) as per the spec is not yet directly supported.



# Variable Types

## Basic

- `string`
- `array `
- `bool`
- `int`

When rendering the spec these variable types are treated as their standard variable type.

## Objects

- `object`
- `object[]`
- `object[k,v]`

When rendering the spec these variable types are recursed. An `object` is recursed. An `object[]` is a treated as a simple array of objects. The ["Paths"](https://spec.openapis.org/oas/v3.1.0.html#paths-object) and any "Map[string, OpenApi Object Type" is handled as a `object[k,v]`.

# Example Usage

```php
$openApi = new OpenApi(
    OpenApi::OPENAPI_VERSION,
    ['title' => 'My Spec\'s title', 'version' => 'My spec\'s version']
);

// add global schemas
foreach ($mySchemas as $mySchema) {
    // $mySchema is your domain logic
    $openApi->getComponents()->addSchema(
        $mySchema->translateToOpenapiPhpLibSchemaName(),
        $mySchema->translateToOpenapiPhpLibSchema()
    );
}

foreach ($myPaths as $myPath) {
    // $myPath is your domain logic
    $pathItem = new PathItem();
    $pathItem->setParameters(
        $myPath->translateToOpenapiPhpLibParameters()
    );

    foreach ($myPath->getSupportedHttpOperations() as $method) {
        $operation = $method->translateToOpenapiPhpLibOperation();

        $pathItem->setOperation(strtolower($method->getHttpMethod()), $operation);
    }
}
```
