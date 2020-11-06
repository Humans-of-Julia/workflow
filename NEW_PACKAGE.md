# Workflow for creating a new Julia package

This workflow is by no mean perfect nor set in stone. Please propose, update, correct it accordingly. 

### Overview

New package should be created/generated following the usual structure of registered Julia packages. Ideally, they should include GitHub automatic actions for **Continuous Integration (CI)**, **Code Coverage**, and **Documentation**. It is a must, unless there is a conflict with other imperative guidelines, to follow Julia's guideline for creating new packages.

Please also consider seriously which License to use. 

## Generating a new package

Packages names can conflict with other if they are registered within the same registry. Please check in advance that the package name is available. For the official Julia registry, please check [JuliaHub](https://juliahub.com/) and follow the Julia package naming [guidelines](https://julialang.github.io/Pkg.jl/dev/creating-packages/#Package-naming-guidelines).

### Generation through [PkgTemplates.jl](https://github.com/invenia/PkgTemplates.jl)

#### First time using PkgTemplates.jl

In the Julia REPL (that you can launch as a standalone or call within a console), please enter Pkg REPL. To quote the package manager documentation:
> Pkg comes with a REPL. Enter the Pkg REPL by pressing ] from the Julia REPL. To get back to the Julia REPL, press backspace or ^C.
 
The following code snippet update the general registry of Julia's packages, then install the `PkgTemplates.jl` package.

```
(@v1.5) pkg> up
(@v1.5) pkg> add PkgTemplates
```

#### Creating a template

Here, we propose a general template for HoJ. It includes CI, coverage, and documentation. Change it accordingly if it not fitting your project.

```julia
using PkgTemplates

HoJ_template = Template(;
           dir="HoJ",  # change if it necessary, it is a temporary folder anyway
           authors="Humans of Julia",
           user="Human-of-Julia"
           julia=v"1.5",
           plugins=[
               License(; name="MIT"),  # change it accordingly
               Git(; manifest=true, ssh=true),
               GitHubActions(; x86=true),
               Codecov(),
               Documenter{GitHubActions}(),
                       Develop(),
           ],
       )
```

#### Generating from the template

Note that if a package with the same name is already added to your julia's current environment, the generation will fail.

```julia
HoJ-template("NameOfMyPackage")
```

*More to come*
