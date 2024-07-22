![GitHub top language](https://img.shields.io/github/languages/top/Ramy-Badr-Ahmed/codemetaCrosswalk)
![GitHub](https://img.shields.io/github/license/Ramy-Badr-Ahmed/codemetaCrosswalk)
[![SWH](https://archive.softwareheritage.org/badge/swh:1:dir:b6b2529f3e58e4af3d8966e3daa61877f4339a8b/)](https://archive.softwareheritage.org/swh:1:dir:b6b2529f3e58e4af3d8966e3daa61877f4339a8b;origin=https://github.com/Ramy-Badr-Ahmed/codemetaCrosswalk;visit=swh:1:snp:8d903de1f5effcbaf4be0d80292eec3006f28264;anchor=swh:1:rev:0cc131e62dd6533e35426e2aae1e4bde2181d7e7)

# Codemeta.json Crosswalk

This repository has been developed as part of the [FAIR4CoreEOSC project](https://faircore4eosc.eu/eosc-core-components/eosc-research-software-apis-and-connectors-rsac) to address two project's pillars (Describe and Cite).

> [!Note]
>  A demonstrable version can be accessed here: [Demo](https://1959e979-c58a-4d3c-86bb-09ec2dfcec8a.ka.bw-cloud-instance.org/)

It currently addresses metadata conversions for the following use cases:

| From     | To            |
|----------|---------------|
| CodeMeta | DataCite [^1] |
| CodeMeta | BibLatex [^2] |
| CodeMeta | BibTex [^3]   |

The codemeta conversion pattern to the above schemes is extendable to other metadata schemes as template classes located under `Schemes` directory. The initial keys correspondence is defined in this repository [^4].

> [!Note]
> There's a scheme template class that can help see this pattern. Please consult the crosswalk [^4] and scheme documentations to properly construct this class.

## Installation Steps:

    1) Clone this project.
    
    2) Open a console session and navigate to the cloned directory:
    
        Run "composer install"

        This should involve installing the PHP REPL, PsySH

    3) (optional) Add psysh to PATH
            
            Example, Bash: 
                    echo 'export PATH="$PATH:/The_Cloned_Directory/vendor/bin"' >> ~/.bashrc
                    source ~/.bashrc

    4) (Optional) Create your local branch.

## Usage:

- In a console session inside the cloned directory, start the php REPL:    

```php
$ psysh     // if not added to PATH replace with: vendor/bin/psysh

Psy Shell v0.12.0 (PHP 8.2.0 — cli) by Justin Hileman
```

- Define namespace:

```php
> namespace Conversions; 
> use Conversions;
```

- Specify codemeta.json path:

```php
> $codeMetaPath = 'CodeMeta/codeMeta.json'
```

> [!Note]
> By default, codemeta.json is located under 'CodeMeta' directory where an example already exists.
> `$codeMetaPath` can also directly take codemeta.json as an array

- Specify target scheme (as fully qualified class name)

```php
> $dataCite = \Schemes\DataCite::class
> $bibLatex = \Schemes\BibLatex::class
> $bibTex   = \Schemes\BibTex::class
```

> [!Note]
> By default, scheme classes are located under 'Schemes' directory.

- Get the conversion from the specified Codemeta.json:

```php
> $errors = NULL;    // initialise errors variable
 
> $dataCiteFromCodeMeta = CodeMetaConversion::To($dataCite, $codeMetaPath, $errors)      // array-formatted

> $bibLatexFromCodeMeta = CodeMetaConversion::To($bibLatex, $codeMetaPath, $errors)      // string-formatted

> $bibTexFromCodeMeta = CodeMetaConversion::To($bibTex, $codeMetaPath, $errors)          // string-formatted
```

- Retrieve errors (if occurred) from the `Illuminate\Support\MessageBag()` instance:

```php
> $errors->messages()      // gets error messages as specified in CodeMeta/conversionsValidations.php

> $errors->keys()          // gets codemeta keys where errors occurred

> $errors->first()         // gets the first occurred error message

> $errors->has('identifier')    // checks whether an error has occurred in the codemeta `identifier` key
```

> [!Note]
> Validations use the `Illuminate\Validation\Validator` package.
> Error messages and rules can be customised in `CodeMeta/conversionsValidations.php` as per the package syntax.


### References
[^1]: [DataCite Metadata Schema](https://schema.datacite.org/meta/kernel-4.3/doc/DataCite-MetadataKernel_v4.3.pdf).
[^2]: [BibLATEX style extension for Software](https://ctan.math.washington.edu/tex-archive/macros/latex/contrib/biblatex-contrib/biblatex-software/software-biblatex.pdf).
[^3]: [BibTex](https://en.wikipedia.org/wiki/BibTeX).
[^4]: [Codemeta Crosswalk](https://github.com/codemeta/codemeta/blob/master/crosswalk.csv).        
