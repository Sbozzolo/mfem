<p align="center">
<a href="https://mfem.org/"><img alt="mfem" src="https://mfem.org/img/logo-300.png"></a>
</p>

<p align="center">
<a href="https://github.com/mfem/mfem/blob/master/LICENSE"><img alt="License" src="https://img.shields.io/badge/License-BSD-brightgreen.svg"></a>
<a href="https://github.com/mfem/mfem/actions?query=workflow%3Arepo-check+branch%3Amaster"><img alt="Repo check" src="https://github.com/mfem/mfem/actions/workflows/repo-check.yml/badge.svg?branch=master"></a>
<a href="https://github.com/mfem/mfem/actions?query=workflow%3Abuild-analysis+branch%3Amaster"><img alt="Build Analysis" src="https://github.com/mfem/mfem/actions/workflows/mfem-analysis.yml/badge.svg?branch=master"></a>
<a href="https://github.com/mfem/mfem/actions?query=workflow%3Abuilds-and-tests+branch%3Amaster"><img alt="Builds and Tests" src="https://github.com/mfem/mfem/actions/workflows/builds-and-tests.yml/badge.svg?branch=master"></a>
<a href="https://ci.appveyor.com/project/mfem/mfem"><img alt="Build Status" src="https://ci.appveyor.com/api/projects/status/19non9sqm6msi2wy?svg=true"></a>
<a href="https://docs.mfem.org/html/index.html"><img alt="Doxygen" src="https://img.shields.io/badge/code-documented-brightgreen.svg"></a>
</p>


# How to Contribute

The MFEM team welcomes contributions at all levels: bugfixes; code improvements;
simplifications; new mesh, discretization or solver capabilities; improved
documentation; new examples and miniapps; HPC performance improvements; etc.

MFEM is distributed under the terms of the BSD-3 license. All new contributions
must be made under this license.

Note also that MFEM has a [Code of Conduct](CODE_OF_CONDUCT.md). By participating
in the MFEM community, you agree to abide by its rules.

If you plan on contributing to MFEM, consider reviewing the
[issue tracker](https://github.com/mfem/mfem/issues) first to check if a thread
already exists for your desired feature or the bug you ran into. Use a pull
request (PR) toward the `mfem:master` branch to propose your contribution. If
you are planning significant code changes or have questions, you may want to
open an [issue](https://github.com/mfem/mfem/issues) before issuing a PR. In
addition to technical contributions, we are also interested in your results and
[simulation images](https://mfem.org/gallery/), which you can share via a pull
request in the [mfem/web](https://github.com/mfem/web) repo.

See the [Quick Summary](#quick-summary) section for the main highlights of our
GitHub workflow. For more details, consult the following sections and refer
back to them before issuing pull requests:

- [Quick Summary](#quick-summary)
- [Code Overview](#code-overview)
- [GitHub Workflow](#github-workflow)
  - [MFEM Organization](#mfem-organization)
  - [New Feature Development](#new-feature-development)
  - [Developer Guidelines](#developer-guidelines)
  - [Pull Requests](#pull-requests)
  - [MFEM PR Rules](#mfem-pr-rules)
  - [Pull Request Checklist](#pull-request-checklist)
  - [Master/Next Workflow](#masternext-workflow)
  - [Releases](#releases)
  - [Release Checklist](#release-checklist)
- [LLNL Workflow](#llnl-workflow)
- [Automated Testing](#automated-testing)
- [Contact Information](#contact-information)

Contributing to MFEM requires knowledge of Git and, likely, finite elements. If
you are new to Git, see the [GitHub learning
resources](https://help.github.com/articles/git-and-github-learning-resources/).
To learn more about the finite element method, see our [FEM page](https://mfem.org/fem).

*By submitting a pull request, you are affirming the [Developer's Certificate of
Origin](#developers-certificate-of-origin-11) at the end of this file.*


## Quick Summary

- We encourage you to [join the MFEM organization](#mfem-organization) and create
  development branches off `mfem:master`.
- Please follow the [developer guidelines](#developer-guidelines), in particular
  with regards to documentation and code styling.
- Please do not commit large/binary files to the central repository (use a fork
  instead).
- Pull requests should be issued toward `mfem:master`. Make sure
  to check the items off the [Pull Request Checklist](#pull-request-checklist) and
  follow the [MFEM PR Rules](#mfem-pr-rules).
- When your contribution is fully working and ready to be reviewed, add
  the `ready-for-review` label.
- PRs are treated similarly to journal submission with an "editor" assigning two
  reviewers to evaluate the changes.
- The reviewers have 3 weeks to evaluate the PR and work with the author to
  fix issues and implement improvements.
- During review there should be no force pushes/rewriting history in the branch.
- After approval, MFEM developers merge the PR manually in the [mfem:next branch](#masternext-workflow).
- After a week of testing in `mfem:next`, the original PR is merged in `mfem:master`.
- We use [milestones](https://github.com/mfem/mfem/milestones) to coordinate the
  work on different PRs toward a release.
- Don't hesitate to [contact us](#contact-information) if you have any questions.


### Code Overview

#### Source code structure:

The MFEM library uses object-oriented design principles which reflect, in code,
the independent mathematical concepts of meshing, linear algebra and finite
element spaces and operators.

The MFEM source code has the following structure:

```
  .
  ├── config
  │   ├── cmake
  │   ├── docker
  │   ├── githooks
  │   └── vcpkg
  ├── data
  ├── doc
  ├── examples
  │   ├── amgx
  │   ├── caliper
  │   ├── ginkgo
  │   ├── hiop
  │   ├── jupyter
  │   ├── moonolith
  │   ├── petsc
  │   ├── pumi
  │   ├── sundials
  |   └── superlu
  ├── fem
  │   ├── ceed
  │   ├── dfem
  │   ├── eltrans
  │   ├── fe
  │   ├── gslib
  │   ├── integ
  │   ├── lor
  │   ├── moonolith
  │   ├── qinterp
  │   └── tmop
  ├── general
  ├── linalg
  │   ├── batched
  │   └── simd
  ├── mesh
  │   └── submesh
  ├── miniapps
  │   ├── adjoint
  │   ├── autodiff
  │   ├── common
  │   ├── dfem
  │   ├── dpg
  │   ├── electromagnetics
  │   ├── gslib
  │   ├── hdiv-linear-solver
  │   ├── hooke
  │   ├── meshing
  │   ├── mtop
  │   ├── multidomain
  │   ├── navier
  │   ├── nurbs
  │   ├── parelag
  │   ├── performance
  │   ├── shifted
  │   ├── solvers
  │   ├── spde
  │   ├── tools
  │   ├── toys
  │   └── tribol
  └── tests
      ├── benchmarks
      ├── convergence
      ├── gitlab
      ├── mem_manager
      ├── par-mesh-format
      ├── scripts
      └── unit
```

#### Main directories and classes

The main directories are `fem/`, `mesh/` and `linalg/` containing the C++
classes implementing the finite element, mesh and linear algebra concepts
respectively.

- The main mesh classes are:
  + [`Mesh`](https://docs.mfem.org/html/classmfem_1_1Mesh.html)
  + [`NCMesh`](https://docs.mfem.org/html/classmfem_1_1NCMesh.html)
  + [`Element`](https://docs.mfem.org/html/classmfem_1_1Element.html)
  + [`ElementTransformation`](https://docs.mfem.org/html/classmfem_1_1ElementTransformation.html)

- The main finite element classes are:
  + [`FiniteElement`](https://docs.mfem.org/html/classmfem_1_1FiniteElement.html)
  + [`FiniteElementCollection`](https://docs.mfem.org/html/classmfem_1_1FiniteElement.html)
  + [`FiniteElementSpace`](https://docs.mfem.org/html/classmfem_1_1FiniteElementSpace.html)
  + [`GridFunction`](https://docs.mfem.org/html/classmfem_1_1GridFunction.html)
  + [`BilinearFormIntegrator`](https://docs.mfem.org/html/classmfem_1_1BilinearFormIntegrator.html) and [`LinearFormIntegrator`](https://docs.mfem.org/html/classmfem_1_1LinearFormIntegrator.html)
  + [`LinearForm`](https://docs.mfem.org/html/classmfem_1_1LinearFormIntegrator.html), [`BilinearForm`](https://docs.mfem.org/html/classmfem_1_1BilinearForm.html) and [`MixedBilinearForm`](https://docs.mfem.org/html/classmfem_1_1MixedBilinearForm.html)

- The main linear algebra classes and sources are
  + [`Operator`](https://docs.mfem.org/html/classmfem_1_1Operator.html) and [`BilinearForm`](https://docs.mfem.org/html/classmfem_1_1BilinearForm.html)
  + [`Vector`](https://docs.mfem.org/html/classmfem_1_1BilinearForm.html) and [`LinearForm`](https://docs.mfem.org/html/classmfem_1_1LinearForm.html)
  + [`DenseMatrix`](https://docs.mfem.org/html/classmfem_1_1DenseMatrix.html) and [`SparseMatrix`](https://docs.mfem.org/html/classmfem_1_1SparseMatrix.html)
  + Sparse [smoothers](https://docs.mfem.org/html/sparsesmoothers_8hpp.html) and linear [solvers](https://docs.mfem.org/html/solvers_8hpp.html)

#### Parallel implementation

Parallel MPI objects in MFEM inherit their serial counterparts, so a parallel
mesh for example is just a serial mesh on each task plus the information on
shared geometric entities between different tasks. The parallel source files
have a `p` prefix, e.g. `pmesh.cpp` vs. the serial `mesh.cpp`.

- The main parallel classes are
  + [`ParMesh`](https://docs.mfem.org/html/solvers_8hpp.html)
  + [`ParNCMesh`](https://docs.mfem.org/html/classmfem_1_1ParMesh.html)
  + [`ParFiniteElementSpace`](https://docs.mfem.org/html/classmfem_1_1ParFiniteElementSpace.html)
  + [`ParGridFunction`](https://docs.mfem.org/html/classmfem_1_1ParGridFunction.html)
  + [`ParBilinearForm`](https://docs.mfem.org/html/classmfem_1_1ParBilinearForm.html) and [`ParLinearForm`](https://docs.mfem.org/html/classmfem_1_1ParLinearForm.html)
  + [`HypreParMatrix`](https://docs.mfem.org/html/classmfem_1_1HypreParMatrix.html) and [`HypreParVector`](https://docs.mfem.org/html/classmfem_1_1HypreParVector.html)
  + [`HypreSolver`](https://docs.mfem.org/html/classmfem_1_1HypreSolver.html) and other [hypre classes](https://docs.mfem.org/html/hypre_8hpp.html)

#### GPU and general device support

GPU and multi-core CPU support is based on device kernels supporting different
backends (CUDA, OCCA, RAJA, OpenMP, etc.) and an internal lightweight
device/host memory manager.

- The main device-relevant classes and sources are:
  + [`Device`](https://docs.mfem.org/html/device_8hpp.html)
  + [`MemoryManager`](https://docs.mfem.org/html/mem_manager_8hpp.html)
  + the [`mfem::forall`](https://docs.mfem.org/html/forall_8hpp.html) function
  + the [`cuda.hpp`](https://docs.mfem.org/html/cuda_8hpp.html) and [`occa.hpp`](https://docs.mfem.org/html/occa_8hpp.html) files

#### Utilities, building and documentation
- The `general/` directory contains C++ classes that serve as utilities for
  communication, error handling, arrays, (Boolean) tables, timing, etc.
- The `config/` directory contains build-related files, both for the plain
  Makefile and the CMake build options.
- The `doc/` directory contains configuration for the Doxygen code documentation
  that can either be built locally or browsed online at https://docs.mfem.org.

#### Examples and tests
- `examples` and `miniapps` respectively gather simple and more fully-featured
  demonstrations of the usage on MFEM. They both rely on `data/` for the
  collection of meshes.
- The `tests/` directory contains a unit test suite and will later contain more
  tests that run example codes.

See also the [code overview](https://mfem.org/code-overview/) section on the MFEM
website.

## GitHub Workflow

The MFEM GitHub organization: https://github.com/mfem, is the main developer hub
for the MFEM project.

If you plan to make contributions or would like to stay up-to-date with changes
in the code, *we strongly encourage you to [join the MFEM organization](#mfem-organization)*.

This will simplify the workflow (by providing you additional permissions), and
will allow us to reach you directly with project announcements.


### MFEM Organization

#### Getting started (GitHub)
Before you can start, you need a GitHub account, here are a few suggestions:
  + Create the account at: [github.com/join](https://github.com/join).
  + For easy identification, please add your full name and maybe a picture of you at:
    https://github.com/settings/profile.
  + To receive notification, set a primary email at: https://github.com/settings/emails.
  + For password-less pull/push over SSH, add your SSH keys at: https://github.com/settings/keys.

#### Joining
- [Contact us](#contact-information) for an invitation to join the MFEM GitHub
  organization. You will receive an invitation email, which you can directly accept.
  Alternatively, *after logging into GitHub*, you can accept the invitation at
  the top of https://github.com/mfem.
- Consider making your membership public by going to https://github.com/orgs/mfem/people
  and clicking on the organization visibility drop box next to your name.
- Project discussions and announcements will be posted at
  https://github.com/orgs/mfem/teams/everyone.

#### Structure
- The MFEM source code is in the [mfem](https://github.com/mfem/mfem)
  repository.
- The website and corresponding documentation are in the
  [web](https://github.com/mfem/web) repository.
- The [PyMFEM](https://github.com/mfem/PyMFEM) repository contains a Python
  wrapper for MFEM.
- The [data](https://github.com/mfem/data) repository contains additional
  (large) datafiles for MFEM.


### New Feature Development

- A new feature should be important enough that at least one person, the
  author, is willing to work on it and be its champion.

- The author creates a branch for the new feature (with suffix `-dev`), off
  the `master` branch, or another existing feature branch, for example:

  ```
  # Clone assuming you have setup your ssh keys on GitHub:
  git clone git@github.com:mfem/mfem.git

  # Alternatively, clone using the "https" protocol:
  git clone https://github.com/mfem/mfem.git

  # Create a new feature branch starting from "master":
  git checkout master
  git pull
  git checkout -b feature-dev

  # Work on "feature-dev", add local commits
  # ...

  # (One time only) push the branch to github and setup your local
  # branch to track the github branch (for "git pull"):
  git push -u origin feature-dev

  ```

- **We prefer that you create the new feature branch inside the MFEM organization
  as opposed to in a fork.** This allows everyone in the community to collaborate
  in one central place.

  - If you prefer to work in your fork, please [enable upstream edits](https://help.github.com/articles/allowing-changes-to-a-pull-request-branch-created-from-a-fork/).

  - Never use the `next` branch to start a new feature branch!

- The typical feature branch name is `new-feature-dev`, e.g. `pumi-dev`. While
  not frequent in MFEM, other suffixes are possible, e.g. `-fix`, `-doc`, etc.


### Developer Guidelines

- *Keep the code lean and as simple as possible*
  - Well-designed simple code is frequently more general and powerful.
  - Lean code base is easier to understand by new collaborators.
  - New features should be added only if they are necessary or generally useful.
  - Introduction of language constructions not currently used in MFEM should be
    justified and generally avoided (to maintain portability to various systems
    and compilers, including early access hardware).
  - We prefer basic C++ and the C++03 standard, to keep the code readable by
    a large audience and to make sure it compiles anywhere.

- *Keep the code general and reasonably efficient*
  - The main goal is fast prototyping for research and application development.
  - When in doubt, generality wins over efficiency.
  - Respect the needs of different users (current and/or future).

- *Keep things separate and logically organized*
  - General usage features go in MFEM (implemented in as much generality as
    possible), non-general features go into external apps.
  - Inside MFEM, compartmentalize between linalg, fem, mesh, GLVis, etc.
  - Contributions that are project-specific or have external dependencies are
    allowed (if they are of broader interest), but should be `#ifdef`-ed and not
    change the code by default.

- Code specifics
  - All new public, protected, and private classes, methods, data members, and
    functions have Doxygen-style documentation in source comments.
  - Math formulas can be included in the Doxygen comments either with standard
    LaTeX (`$..$` and `$$..$$`) for portions that need detailed explanation, or with
    [Unicode](https://www.unicodeit.net/) or plain text when short or readable description is preferable.
  - In addition to arguments and functionality, documentation should include the
    current limitations of the code, any background information that is
    implicitly assumed in the implementation, and the ownership and lifetime
    of data.
  - Consistent code styling is enforced with `make style` in the top-level
    directory. This requires [Artistic Style](http://astyle.sourceforge.net) (we
    specifically use version 3.1). See also the file `config/mfem.astylerc`.
  - Use `mfem::out` and `mfem::err` instead of `std::cout` and `std::cerr` in
    internal library code. (You can use `std` in examples and miniapps.)
  - When manually resolving conflicts during a merge, make sure to mention the
    conflicted files in the commit message.
  - All significant new features and changes should be documented in CHANGELOG.
  - New examples and miniapps should have documentation on the MFEM webpage.
  - The general floating-point type `real_t` should be used, rather than
    `float` or `double`, except in special cases where only one is possible.


### Pull Requests

- When your branch is ready for other developers to review / comment on
  the code, create a pull request towards `mfem:master`.

- Pull request typically have titles like:

     `Description [new-feature-dev]`

  for example:

     `Parallel Unstructured Mesh Infrastructure (PUMI) integration [pumi-dev]`

  Note the branch name suffix (in square brackets).

- Titles may contain a prefix in square brackets to emphasize the type of PR.
  Common choices are: `[DON'T MERGE]`, `[WIP]` and `[DISCUSS]`, for example:

     `[DISCUSS] Hybridized DG [hdg-dev]`

- If the PR is still a work in progress, add the `WIP` label to it and
  optionally the `[WIP]` prefix in the title.

- Add a description, appropriate labels and assign yourself to the PR. The MFEM
  team will add reviewers as appropriate.

- List outstanding TODO items in the description, see PR #222 for an example.

- When your contribution is fully working and ready to be reviewed, add
  the `ready-for-review` label.

- PRs are treated similarly to journal submission with an "editor" assigning
  two reviewers to evaluate the changes. The reviewers have 3 weeks to evaluate
  the PR and work with the author to implement improvements and fix issues.

- Once the `ready-for-review` label has been applied and reviewers have been
  assigned, the PR is considered under review. To help with the review process
  there should be no force pushes/rewriting history in the branch.

- After approval, the PR is [tested](#masternext-workflow) for a week with
  other approved PRs in the `mfem:next` branch.

- Consider manually running the tests in `tests/scripts` before merging in
  `mfem:next`, see the [README](tests/scripts/README) file in that directory
  for more details.

- Track the GitHub Actions and Appveyor [continuous integration](#automated-testing)
  builds at the end of the PR. These should generally run clean, so address any
  errors as soon as possible. Please ask if you are unsure how to do that.

- Note that some tests, such as the `branch-history` check in GitHub Actions
  are safeguards that are allowed to fail in certain cases.

- Other tests, such as the `code-style`, `documentation` and `gitignore`
  checks in GitHub Actions enforce MFEM-specific rules which are explained in
  the error messages and the `tests/scripts` directory.

- Also note that the tests `branch-history` and `repos-checks` found in GitHub
  Actions can be triggered automatically before each push using git hooks. See
  the [git hooks README](config/githooks/README.md) for a detailed explanation.

- If triggered, track the status of the LLNL GitLab tests. If failing, ask
  one of the _LLNL developers_ for details.


### MFEM PR Rules

The Pull Request (PR) approval process in MFEM is similar to the approval of papers in a peer-reviewed journal. In particular:

1. There is an MFEM board of "editors" that evaluates new PRs and assigns "reviewers" for each PR.

2. The assigned reviewers are responsible to carefully review and test the proposed PR.

3. A PR can be (manually) merged in the *next* branch only if 2 of the assigned reviewers have approved it and it has passed internal testing. This merge can be performed by any of the assigned reviewers or by any of the editors.

4. A PR can be merged in the *master* branch only if it has been tested successfully for a week in *next* and an editor has (optionally) taken a final look. This merge can be performed only by one of the editors.

#### Responsibilities of Editors

The current list of MFEM editors is:

- [@v-dobrev](https://github.com/v-dobrev) (Veselin Dobrev)
- [@tzanio](https://github.com/tzanio) (Tzanio Kolev)
- [@pazner](https://github.com/pazner) (Will Pazner)
- [@mlstowell](https://github.com/mlstowell) (Mark Stowell)

**The responsibilities of the editors are:**

1. To assign appropriate milestone and labels for new PRs, e.g. *bugfix*, *minor*, *api-change*, *high-impact*, etc.

2. To assign at least 2 reviewers for new PRs. An editor can also be a reviewer. The editor, reviewers, and author should be listed as "Assignees" on the GitHub PR page. After assignment, the `in-review` label should be added.

3. To complete the initial PR evaluation and assignments in a timely manner: 1 week from submission.

4. To assist reviewers when they need help with their reviews (but also to stay out of the way when they don't).

5. To remind the reviewers about timely completion of their review.

6. To take a final look and complete the PR merge in *master*. The final look step is optional and shouldn't take more than 3 days.

7. The assignment of bugfixes should be expedited proportional to their importance, e.g. in some cases the editor can assign much shorter review window.

#### Responsibilities of Reviewers

Everyone on the MFEM team can be asked to serve as a reviewer on a PR in their area of expertise.

**The responsibilities of the reviewers are:**

1. To let the editors know if the proposed assignment is not a good match for them.

2. To communicate with the PR author, provide feedback and work with them to resolve issues.

3. To ensure the quality of the PR by making sure that the code adheres to the [Developer Guidelines](#developer-guidelines), e.g. all methods, data members, and functions have documentation, including data ownership and lifetime, new examples/miniapps have a corresponding PR in mfem/web, major features have `CHANGELOG` entries, etc.

3. To seek help from the editors in case of difficulties.

4. To complete the review in a timely manner: 3 weeks from assignment.

5. To test the PR thoroughly before merging in *next*. The PR author is also encouraged to perform testing and inform the reviewers about the results.

6. To monitor the PR impact on the testing in the *next* branch and alert the editors that the PR is ready for merging in *master*.

7. The review of bugfixes should be expedited proportional to their importance. The review window can be much less than three weeks in such cases.

#### Responsibilities of Authors

Authors should clearly indicate when a PR is ready for review (before that the PR should be marked as `Draft` or `[WIP]`).

**The responsibilities of the authors are:**

1. To follow the instructions and PR checklist in the `CONTRIBUTING.md` document in the MFEM repository.

2. To respond to reviewer feedback in a timely manner.

3. Authors are encouraged to perform testing and inform the reviewers about the results.

4. Authors can use the "Reviewers" section of the GitHub PR page to suggest reviewers, but the "Assignees" section will show who the editor has assigned to do the reviews.

5. To indicate when the PR is ready for review by adding the `ready-for-review` label.


### Pull Request Checklist

Before a PR can be merged, it should satisfy the following:

- [ ] Code builds.
- [ ] Code passes `make style`.
- [ ] Update `CHANGELOG`:
    - [ ] Is this a new feature users need to be aware of? New or updated example or miniapp?
    - [ ] Does it make sense to create a new section in the `CHANGELOG` to group with other related features?
- [ ] Update `INSTALL`:
    - [ ] Had a new optional library been added? If so, what range of versions of this library are required? (*Make sure the external library is compatible with our BSD license, e.g. it is not licensed under GPL!*)
    - [ ] Have the version ranges for any required or optional libraries changed?
    - [ ] Does `make` or `cmake` have a new target?
    - [ ] Did the requirements or the installation process change? *(rare)*
- [ ] Update continuous integration server configurations if necessary (e.g. with new version requirements for each of MFEM's dependencies)
    - [ ] `.github`
    - [ ] `.appveyor.yml`
- [ ] Update `.gitignore`:
    - [ ] Check if `make distclean; git status` shows any files that were generated from the source by the project (not an IDE) but we don't want to track in the repository.
    - [ ] Add new patterns (just for the new files above) and re-run the above test.
- [ ] New examples:
    - [ ] All sample runs at the top of the example source file work.
    - [ ] Update `examples/makefile`:
      - [ ] Add the example code to the appropriate `SEQ_EXAMPLES` and `PAR_EXAMPLES` variables.
      - [ ] Add any files generated by it to the `clean` target.
      - [ ] Add the example binary and any files generated by it to the top-level `.gitignore` file.
    - [ ] Update `examples/CMakeLists.txt`:
      - [ ] Add the example code to the `ALL_EXE_SRCS` variable.
      - [ ] Make sure `THIS_TEST_OPTIONS` is set correctly for the new example.
   - [ ] List the new example in `doc/CodeDocumentation.dox`.
   - [ ] If new examples directory (e.g.`examples/pumi`), list it in `doc/CodeDocumentation.conf.in`
   - [ ] Companion pull request for documentation in [mfem/web](https://github.com/mfem/web) repo:
      - [ ] Update or add example-specific documentation, see e.g. the `src/examples.md`.
      - [ ] Add the description, labels and screenshots in `src/examples.md` and `src/img`.
      - [ ] In `examples.md`, list the example under the appropriate categories, add new categories if necessary.
      - [ ] Add a short description of the example in the "Extensive Examples" section of `features.md`.
- [ ] New miniapps:
   - [ ] All sample runs at the top of the miniapp source file work.
   - [ ] Add to internal testing repo, if sample runs should be included in nightly tests [internally](#tests-at-llnl).
   - [ ] Exclude long sample runs from automated testing, with `* ` (one space) before the command.
   - [ ] Update top-level `makefile` and `makefile` in corresponding miniapp directory.
   - [ ] Add the miniapp binary and any files generated by it to the top-level `.gitignore` file.
   - [ ] Update CMake build system:
      - [ ] Update the `CMakeLists.txt` file in the `miniapps` directory, if the new miniapp is in a new directory.
      - [ ] Add/update the `CMakeLists.txt` file in the new miniapp directory.
      - [ ] Consider adding a new test for the new miniapp.
   - [ ] List the new miniapp in `doc/CodeDocumentation.dox`
   - [ ] If new miniapps directory (e.g.`miniapps/nurbs`), add it to `MINIAPP_SUBDIRS` in the `makefile`.
   - [ ] If new miniapps directory (e.g.`miniapps/nurbs`), list it in `doc/CodeDocumentation.conf.in`
   - [ ] Companion pull request for documentation in [mfem/web](https://github.com/mfem/web) repo:
     - [ ] Update or add miniapp-specific documentation, see e.g. the `src/meshing.md` and `src/electromagnetics.md` files.
     - [ ] Add the description, labels and screenshots in `src/examples.md` and `src/img`.
     - [ ] The miniapps go at the end of the page, and are usually listed only under a specific "Application (PDE)" category.
     - [ ] Add a short description of the miniapp in the "Extensive Examples" section of `features.md`.
- [ ] New capability:
   - [ ] All new public, protected, and private classes, methods, data members, and functions have full Doxygen-style documentation in source comments. Documentation should include descriptions of member data, function arguments and return values, template parameters, and prerequisites for calling new functions.
   - [ ] Pointer arguments and return values must specify whether ownership is being transferred or lent with the call.
   - [ ] Any new functions should include descriptions of their intended use e.g. for internal use only, user-facing, etc., along with references to example code whenever possible/appropriate.
   - [ ] Consider adding new sample runs in existing examples to highlight the new capability.
   - [ ] Consider saving cool simulation pictures with the new capability in the Confluence gallery (LLNL only) or submitting them, via pull request, to the gallery section of the `mfem/web` repo.
   - [ ] If this is a major new feature, consider mentioning it in the short summary inside `README` *(rare)*.
   - [ ] List major new classes in `doc/CodeDocumentation.dox` *(rare)*.
- [ ] Update this checklist, if the new pull request affects it.
- [ ] Run `make unittest` to make sure all unit tests pass.
- [ ] Run the tests in `tests/scripts`.
- [ ] (LLNL only) After merging:
   - [ ] Update internal tests to include the new features.


### Master/Next Workflow

MFEM uses a `master`/`next`-branch workflow as described below:

- The `master` branch should always be of release quality and changes should not
  be merged until they have been fully tested. This branch is protected, and
  changes can only be made through pull requests.

- After approval, a pull request is merged manually (by MFEM developers) in the
  `next` branch for testing and the `in-next` label is added to the PR.
  This can be done as follows:

  ```
  # Pull the latest version of the "feature-dev" branch
  git checkout feature-dev
  git pull

  # Pull the latest version of the "next" branch
  git checkout next
  git pull

  # Merge "feature-dev" into "next", resolving conflicts, if necessary.
  # Use the "--no-ff" flag to create a new commit with merge message.
  git merge --no-ff feature-dev

  # Push the "next" branch to the server
  git push
  ```

- After a week of testing in `next` (excluding bugfixes), both on GitHub, as
  well as [internally](#tests-at-llnl) at LLNL, the original PR is merged into
  `master` (provided there are no issues).

- After the merge, the feature branch is deleted (unless it is a long-term
  project with periodic PRs).

- The `next` branch is used just for integrated testing of all PRs approved for
  merging into `master` to verify that each works individually and that all of
  them work as a group. This branch can be discarded at any time, though we
  typically do that only at the end of a [release cycle](#releases).


### Releases

- Releases are just tags in the `master` branch, e.g. https://github.com/mfem/mfem/releases/tag/v3.3.2,
  and have a version that ends in an even "patch" number, e.g. `v3.2.2` or
  `v3.4` (by convention `v3.4` is the same as `v3.4.0`.)  Between releases, the
  version ends in an odd "patch" number, e.g. `v3.3.3`.

- We use [milestones](https://github.com/mfem/mfem/milestones) to coordinate the
  work on different PRs toward a release, see for example the
  [v3.3.2 release](https://github.com/mfem/mfem/milestone/1?closed=1).

- After a release is complete, the `next` branch is recreated, e.g. as follows
  (replace `3.3.2` with current release):
  - Rename the current `next` branch to `next-pre-v3.3.2`.
  - Create a new `next` branch starting from the `v3.3.2` release.
  - Local copies of `next` can then be updated with `git fetch origin next && git checkout -B next origin/next`.

### Release Checklist

- [ ] Update the MFEM version in the following files:
    - [ ] `CHANGELOG`
    - [ ] `makefile`
    - [ ] `CMakeLists.txt`
    - [ ] `doc/CodeDocumentation.conf.in`
- [ ] Check that version requirements for each of MFEM's dependencies are documented in `INSTALL` and up-to-date
- [ ] Check that continuous integration server configurations reflect the dependency version requirements of the new release
    - [ ] `.github`
    - [ ] `.appveyor.yml`
- [ ] Update the `CHANGELOG` to organize all release contributions
- [ ] Review the whole source code once over
- [ ] Ask MFEM-based applications to test the pre-release branch
- [ ] Test on additional platforms and compilers
- [ ] Tag the repository:

  ```
  git tag -a v3.1 -m "Official release v3.1"
  git push origin v3.1
  ```
- [ ] Create the release tarball and push to `mfem/releases`.
- [ ] Recreate the `next` branch as described in previous section.
- [ ] Update and push documentation to `mfem/doxygen`. Update the `README.md` file and the `html` link in the `mfem/doxygen` repo.
- [ ] Update URL shortlinks:
    - [ ] Create a shortlink at [http://bit.ly/](http://bit.ly/) for the release tarball, e.g. https://mfem.github.io/releases/mfem-3.1.tgz.
    - [ ] (LLNL only) Add and commit the new shortlink in the `links` and `links-mfem` files of the internal `mfem/downloads` repo.
    - [ ] Add the new shortlinks to the MFEM packages in `spack`, `homebrew/science`, `VisIt`, etc.
- [ ] Update website in `mfem/web` repo:
    - Update version and shortlinks in `src/index.md` and `src/download.md`.
    -  Use [cloc-1.62.pl](http://cloc.sourceforge.net/) and `ls -lh` to estimate the SLOC and the tarball size in `src/download.md`.


## LLNL Workflow


### Mirroring on Bitbucket

- The GitHub `master` and `next` branches are mirrored to the LLNL institutional
  Bitbucket repository as `gh-master` and `gh-next`.

- `gh-master` is merged into LLNL's internal `master` through pull requests; write
  permissions to `master` are restricted to ensure this is the only way in which it
  gets updated.

- We never push directly from LLNL to GitHub.

- Versions of the code on LLNL's internal server, from most to least stable:
  - MFEM official release on mfem.org -- Most stable, tested in many apps.
  - `mfem:master` -- Recent development version, guaranteed to work.
  - `mfem:gh-master` -- Stable development version, passed testing, you can use
     it to build your code between releases.
  - `mfem:gh-next` -- Bleeding-edge development version, may be broken, use at
     your own risk.


### Mirroring on GitLab

- MFEM repository is also mirrored on the LLNL GitLab instance, in a
  semi-automated manner.

- This instance is meant to complete CI testing with tests on Livermore
  Computing systems. GitLab pipeline status is reported in the corresponding
  GitHub pull request.

- In GitLab pipelines, TPLs (dependencies) are built using Spack, driven by Uberenv.

- No change to the MFEM repo can be made on this instance.

## Automated Testing

MFEM has several levels of automated testing running on GitHub, as well as on
local Mac and Linux workstations, and Livermore Computing clusters at LLNL.

In addition, developers can set local git hooks to run some quick checks on
commit or push, see the [README](config/githooks/README.md) in the `config/githooks`
directory.


### Linux and Mac smoke tests
We use GitHub Actions to drive the default tests on the `master` and `next`
branches. See the `.github/workflows` files and the logs at
[https://github.com/mfem/mfem/actions](https://github.com/mfem/mfem/actions).

Testing using GitHub Actions should be kept lightweight, as there is a time
constraint on jobs. Two virtual machines are configured - Mac (OS X) and Linux.

- Tests on the `master` branch are triggered whenever a PR is issued on this branch.
- Tests on the `next` branch are currently scheduled to run each night.


### Windows smoke test
We use Appveyor to test building with the MS Visual C++ compiler in a Windows
environment, as well as to test the CMake build. See the `.appveyor` file and the
build logs at
[https://ci.appveyor.com/project/mfem/mfem](https://ci.appveyor.com/project/mfem/mfem).

CMake is used to generate the MSVC Project files and drive the build.  A release
and debug build is performed with a simple run of `ex1` to verify the executable.


### Tests at LLNL

- We mirror the `master` and `next` branches internally (to `gh-master` and
  `gh-next`) and run longer nightly tests via cron. On the weekends, a more
  extensive test is run which extracts and executes all the different sample
  runs from each example and most miniapps.

- We also mirror PRs on the LLNL GitLab instance. PR mirroring can only be
  triggered by _LLNL developers_, but test status is publicly available. Only
  _LLNL developers_ can access the detailed test report.

## Contact Information

- Contact the MFEM team by posting to the [GitHub issue tracker](https://github.com/mfem/mfem).
  Please perform a search to make sure your question has not been answered already.

- Email communications should be sent to the MFEM developers mailing list,
  mfem-dev@llnl.gov.


## [Developer's Certificate of Origin 1.1](https://developercertificate.org/)

By making a contribution to this project, I certify that:

(a) The contribution was created in whole or in part by me and I have the right
    to submit it under the open source license indicated in the file; or

(b) The contribution is based upon previous work that, to the best of my
    knowledge, is covered under an appropriate open source license and I have
    the right under that license to submit that work with modifications, whether
    created in whole or in part by me, under the same open source license
    (unless I am permitted to submit under a different license), as indicated in
    the file; or

(c) The contribution was provided directly to me by some other person who
    certified (a), (b) or (c) and I have not modified it.

(d) I understand and agree that this project and the contribution are public and
    that a record of the contribution (including all personal information I
    submit with it, including my sign-off) is maintained indefinitely and may be
    redistributed consistent with this project or the open source license(s)
    involved.
