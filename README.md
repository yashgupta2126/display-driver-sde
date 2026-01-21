# pkg-template

This repository serves as a template for creating Debian package repositories within the Qualcomm Linux ecosystem. It provides the essential structure, GitHub workflows, and configuration necessary to integrate with the [qcom-build-utils](https://github.com/qualcomm-linux/qcom-build-utils) repository, enabling standardized Debian package building processes.

For a comprehensive tutorial on utilizing this template with an example project, refer to the [pkg-example README](https://github.com/qualcomm-linux/pkg-example/blob/main/README.md).

## Quick Start

To create a new Debian package repository using this template:

1. Navigate to this repository's GitHub page and click the **"Use this template"** button located in the top right corner.
2. Name the new repository with the prefix `pkg-` to adhere to the naming convention for package repositories.
3. Ensure the **"Include all branches"** option is enabled. Otherwise by default, only the default branch "main" is cloned.

## Branches

- **main**: The primary branch containing workflow logic in the `.github/` folder, along with boilerplate documentation files such as license, contribution guidelines, and this README.
- **debian/qcom-next**: An orphan branch with unrelated history from main. It contains a debian/ folder with template files. Its just to give a starting point and structure. The first job for the user templating from this repo will be to update this debian/ folder. The information about the name **debian/qcom-next** and other naming conventions can be found [here](https://qualcomm-confluence.atlassian.net/wiki/spaces/LinuxCoreOS/pages/2879858691/pkg-+repository+specification)

## Workflows

The `main` branch includes the following workflows in the `.github/workflows/` directory:

- **qcom-preflight-checks.yml**: A sanity check workflow inherited from the base Qualcomm template.
- **stale-issues.yml**: A workflow for managing stale issues, also inherited from the base template.
- **build-debian-package.yml**: Builds the Debian package for this repository. This workflow serves as an entry point that invokes reusable workflows from the centralized qcom-build-utils repository.
- **promote-upstream.yml**: Promotes the package's tracking version to a new upstream release. This workflow also triggers reusable workflows in qcom-build-utils.

## Repository Configuration

### Repository Variables

Set the following repository variables to establish links between upstream and package repositories:

- **UPSTREAM_REPO_GITHUB_NAME**: In this package repo, this is the GitHub name of the upstream repository (e.g. in the case of the pkg-example, `qualcomm-linux/qcom-example-package-source`).
- **PKG_REPO_GITHUB_NAME**: This variable is set in the upstream project repo; the GitHub name of the package repository (e.g., `qualcomm-linux/pkg-example`).

### Branch Protection Rules

Configure branch protection for `debian/qcom-next`:

- Restrict deletions.
- Require pull requests before merging.
- Block force pushes.
- Add `build / build-debian-package` as a required status check.

### Additional Settings

- Ask [Mark Matyas](mmatyas@qti.qualcomm.com) to share the following org secret/variables with the repo :
  - secrets.DEB_PKG_BOT_CI_TOKEN
  - vars.DEB_PKG_BOT_CI_USERNAME
  - vars.DEB_PKG_BOT_CI_EMAIL
- Enable **"Automatically delete head branches"** for pull requests.
- Allow only merge commits for pull request merges.
- Enable **release immutability** in the upstream repository.
- Add the Qualcomm Github Service bot as a user with write access :
  - While the repo is private, add the Github user **qcom-service-bot**.
  - If/when the repo is made public, there will be a big change in how the contributors are handled. 
    After that, the contributors list is cleared, and one need to re-enroll as a contributor. The way
    to do that is completely different from when the repo was private. When it was private, the creator
    of the repo had the possibility to go into the repo settings and add whoever. When made public, the
    repo's maintainer/contributor list is managed by a Qualcomm internal mailing list. If the repo is
    named pkg-foo, then the Maintainer list will be named Maintainers.pkg-foo, and one need to request
    access via https://lists.qualcomm.com, find the list and ask access. For the bot, you must add it via
    its qualcomm username **githubservice**@qti.qualcomm.com, as opposed to its Github handle above.

### AWS Runners

For teams utilizing AWS runners, document the process for setting up and managing these resources as part of the repository configuration.

## Development

Refer to the [CONTRIBUTING.md](CONTRIBUTING.md) file for guidelines on contributing to this project.

## Getting in Contact

For support or inquiries, contact sbeaudoi@qti.qualcomm.com.

## License

pkg-template is licensed under the [BSD-3-Clause License](https://spdx.org/licenses/BSD-3-Clause.html). See [LICENSE.txt](LICENSE.txt) for the full license text.
