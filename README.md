# Z Markup Language (ZML) : Specification

## The Idea

The main idea of the Z Markup Language (ZML) is to have a programming language agnostic macro description format.
Over the last years, several new programming languages became popular.
At the same time, it is difficult to exchange mapping macros between different languages.
Different programming languages have different needs and sometimes the information stored in the macros is not sufficient.
In our current development projects we need additional meta data, which is currently not available in a machine readable format:

* Variable scaling  
  Some SMF variables are scaled (e.g., by 16 or 256) and it is necessary to process the variable before the data can be presented.
  The scaling information is documented in the macro as a comment and the parser can't extract this information.
* Reference to another object  
  Often control blocks use pointers to point to the next control block.
  For example, the pointer ASCBOUCB in the ASCB points to the OUCB control block.
  In this case the variable name describes the object pointed to, but a parser can't extract such information.
* Other  
  There is much more information available, which helps to use map data (in storage or in a dataset) in a programmatic way.
  Other metadata information is the _time format_, _array information_, _official programming interface_ and more.

The ZML Specification tries to addresses the following problems:

* Control block mappings exchange between different programming languages
* Mapping of raw data in control blocks or files to structures
* Programmatic code generation to display raw data on a terminal

The [ZML Specification](./ZML-Specification/draft/ZML-Specification.adoc) description is based on the [OpenAPI Specification](http://spec.openapis.org/oas/v3.0.3), which defined a language-agnostic interface to HTTP APIs

## How it Works

A ZML converter transforms existing macros into YAML file (the ZML document).
After the conversion, the generated ZML document contains field names and the tags, which the converter could identify.
Each field name has several tags like offset, type, description ..., but additional meta data tags are not available.

**Note:** The additional meta tags are only available, when a macro is instrumented with parsing tags, which allow to generate  a specific ZML tag.

Here a HL Assembler variable declaration with an instrumentation sample:
````nasm
P2Z_Reference DS A      31 bit pointer with a local reference
                        to the P2Z DSECT
                        x-[asm]-reference(p2z,p2zasm)
````
and the resulting field in the ZML Document looks as follows:
````yaml
P2Z_Reference:
    type: integer
    format: ptr31
    description: >
        31 bit pointer with a local reference to
        the P2Z DSECT  
    x-zml-offset: 16
    x-zml-size: 4
    x-zml-schema:
        $ref: "p2zasm.yaml#/components/schemas/p2z"
````

Finally, a ZML validator validates the document, to ensure that it follows the definition in the [ZML Specification](./ZML-Specification/ZML-Specification.adoc).

## Tooling

* ZML validator
* ZML tooling
* ZML converter  
  Currently no Open Source Converter is available

## Develop ZML Tool

This repo uses [poetry](https://python-poetry.org/docs/) to manage dependencies and packaging.
All Required information can be found on the poetry documentation page.

To validate a ZML file in the terminal run

```sh
poetry run zml validate [zml_file]
```

## Additional Information
