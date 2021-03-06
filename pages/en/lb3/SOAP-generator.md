---
title: "SOAP generator"
lang: en
layout: page
keywords: LoopBack
tags: [authentication]
sidebar: lb3_sidebar
permalink: /doc/en/lb3/SOAP-generator.html
summary:
---

{% include content/generator-create-app.html lang=page.lang %}

### Synopsis

Generate models based on SOAP web service.

```
lb soap [options] [<wsdl_file>]
```

Where `<wsdl_file>` is the URL or local path to a Web Services Description Language (WSDL) file.

For more information and an example, see [Connecting to SOAP web services](Connecting-to-SOAP.html).

### Options

`-h, --help`
Print the generator's options and usage.

`--skip-cache`
Do not remember prompt answers. Default is false.

`--skip-install`
Do not automatically install dependencies. Default is false.

### Interactive Prompts

The tool will prompt you for the necessary information and then modify the [Model definition JSON file](Model-definition-JSON-file.html) accordingly.

The generator prompts for:

- URL or path to WSDL file to use.  The tool will use any value provided on the command-line as the default.
- Service to use from the specified WSDL specification.
- Binding to use from the specified service.
- Operations to generate from the specified binding and service.  You must choose at least one operation.
