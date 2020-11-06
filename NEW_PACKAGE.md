# Workflow for creating a new Julia package

This workflow is by no mean perfect nor set in stone. Please propose, update, correct it accordingly. 

### Overview

New package should be created/generated following the usual structure of registered Julia packages. Ideally, they should include GitHub automatic actions for **Continuous Integration (CI)**, **Code Coverage**, and **Documentation**. It is a must, unless there is a conflict with other imperative guidelines, to follow Julia's guideline for creating new packages.

#### License

I will assume that a new HoJ package will be licensed under an open-source license. If the package managers prefers to publish unlicensed code, please use `The Unlicensed` license (such as in this repository). Otherwise, a simple way to look at things is if the OSS license should be permissive (for instance MIT) or not (GPL). 

To illustrate the difference, let's use the example of `StaticWebPages.jl` and `Bibliography.jl`. The last is a bibliography format manager used here by `StaticWebPages.jl`, among others such as (soon) `Documenter.jl`, to handle bibliography output in a webformat.
- `Biblography.jl` is designed to be used by various software and packages, as such it has a permissive OSS license (MIT). It allows users and companies to use it without worries (unless they decide to publish their code). Please note that anyone building upon this package can use a more restrictive license.
- `StaticWebPages.jl` is under the more restrictive GPLv2 license. It provides a software in its final form that is unlikely to be integrated within another package. Also it contains some web programming content coming from a GPLv2 licensed OSS.

Please consider seriously which License to use. DO not hesitate to ask advice on the discord.

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
           dir="HoJ",  # change if it necessary
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
HoJ_template("NameOfMyPackage")
```

The template will appear in the `dir` directory provided above. It will also be added (as a local package) to your Julia main environment.

### Sync with a GitHub repository

- (optional) Create or join a `NameOfMyProject/Package` team 
- Within HoJ organization, create a `NameOfMyPackage.jl` repository. Keep it empty.
- In the git folder, use `git push origin main`
- (optional) Use `rm NameOfMyPackage` in the Pkg REPL. Follow with `dev git@github.com:Humans-of-Julia/NameOfMyPackage.jl.git`. You can delete the original folder in `dir`.

Although the last step is optional, it allows you to have all the packages you develop in `user(home)\.julia\dev\`.

#### Activate codecoverage (with codecov.io)
* I will update this section eventually, in the mean time, go to codecov.io and follow the instructions ... *

#### Activate the GitHub hosted documentation on (succesfull) push
* I will update this section eventually, in the mean time, you can build your doc manually ... *

### Small tips before publishing the first version on JuliaHub
- Check again if the package name is free. If not, rename everything, including the git repository.
- Do not tag any version prior to the current state of master. Tagging the current version is unecessary (a bot will do it).
- The first version in Project.toml needs to be either: `0.0.1` (kind of alpha), `0.1.0` (kind of beta, most common first version), `1.0.0` (first major stable version).

*More to come, suggestions are also welcome*
