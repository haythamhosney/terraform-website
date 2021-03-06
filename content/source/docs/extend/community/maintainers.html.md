---
layout: "extend"
page_title: "Community"
sidebar_current: "docs-extend-community-maintainers"
description: |-
  Terraform is a mature project with a growing community. There are
  active, dedicated people willing to help you through various mediums.

---

# Provider Maintainers

HashiCorp tools are developed in open source with contributions from a wide variety of community members. A high value is placed on these individual contributions and different members can be grouped by the following definitions:

- Community member: Anyone using Terraform as a practitioner, or extending it as a developer.
- Contributor: Anyone who has added to the code repo, things like issues, votes, reactions, comments, and pull requests. If they have commits merged into the project, they can be recognized with a "contributor" badge on GitHub.
- Collaborator: Someone who has demonstrated the skills, abilities, and care for working on a particular project, such that we have asked and they have accepted responsibilities for oversight/work on a project. For providers maintained by the cloud vendor, these users are selected by the vendor and the might even share most responsibilities of a "maintainer". On GitHub they can be recognized with a "collaborator" badge.
- Maintainer: Includes all previously described responsibilities but also plays a governance role on the project. On GitHub and in the context of Terraform Providers they can be recognized with an "owner" badge.

The overlap between "collaborator" and "maintainer" can fluctuate project to project; in fact, GitHub maintains no such distinction. Some providers have a clear HashiCorp maintainer role with a community of impactful collaborators donating their time to the project. Other providers can be a joint engineering effort between HashiCorp maintainers and outside collaborators the provider vendor has chosen. HashiCorp will generally avoid vetting external maintainers, but will help ensure existing and prospective maintainers have a shared understanding and acceptance of roles and responsibilities.

## Community Guidelines

All community members are responsible for their interactions with the rest of the community, to keep Terraform and provider development successful it's important to follow our [community guidelines](https://www.hashicorp.com/community-guidelines).

## Provider Development Program

Providers in the development program get vetted and hosted in the terraform-providers GitHub organization. Please see [the website](https://www.terraform.io/guides/terraform-provider-development-program.html#checklist) for instructions on following that process.

## Maintainer Responsibilities 

Being a maintainer of a provider involves several responsibilities from triaging issues and pull requests to editing the changelog and requesting for releases to be cut.

### Pull Requests

Pull requests to a provider should feature relevant tests with instructions on running them, or output showing they have passed. For bug fixes, inclusion of repro instructions is helpful.

Maintainers are welcome to assess, reply, and label open issues/pull requests. We typically use a 2-label system of:

1. Type: such as "bug", "enhancement", "documentation", or "question"
2. Service/Section: in the case of AWS something like "service/ec2" or "service/rds"

#### Checklist

Pull requests should cover the following:

- Acceptance test coverage of new behavior: these tests should exercise all the behavior of the resource. Sometimes it is sufficient to "enhance" an existing test by adding an assertion or tweaking the config that is used, but often a new test is better. Tests should also pass with the `TF_SCHEMA_PANIC_ON_ERROR` environment variable set, ensuring the schema is properly configured and not silently failing.
- Documentation updates: All relevant changes (schema, unique behaviors) should be documented and be covered in the same pull request.
- Well-formed code: Ensure code passes linting (go fmt, go vet). Ensure code follows [provider development best practices](https://www.terraform.io/docs/extend/index.html).
- Vendoring updates: Updating the vendor folder should be done in a separate pull request. This is to avoid conflicts as the versions of dependencies tend to be fast moving targets.

### Merging

If a pull request has been approved by a maintainer and the submitter has push privileges (recognizable via Collaborator or Member badge), the submitter should merge their own pull request. Any pull request that significantly changes or breaks user experience should always get at least one vote of approval from a HashiCorp employee prior to merge. We prefer to merge via the GitHub web interface using the green merge button. Squash when the commit history is irrelevant.

### Changelog

Changelogs are an important piece of documentation within a provider. All providers should follow a consistent CHANGELOG that is broken down as follows:

- Backwards incompatibilities: This section documents in brief any incompatible changes and how to handle them. This is only present in certain versions that can have breaking changes, see the subsection of "versioning" for more information.
- Features: These are major new features or major new improvements that deserve a special highlight, such as a new resource or data source like EKS cluster for the AWS provider.
- Improvements: Smaller features or enhancements added to the project such as a new field for a resource.
- Bug fixes: Any bugs that were fixed.

To keep the CHANGELOG up to date, we recommend updating the CHANGELOG after every set of commits that change the project. Never include CHANGELOG updates in a pull request. If you do, it will very likely cause a merge conflict with other pull requests. Instead, make the pull request without CHANGELOG updates, and add to the CHANGELOG only after merge.

#### Entry Format

To keep the CHANGELOG entries standardized across our tools, we follow a simple format scheme as follows:

```
CATEGORY: [BACKWARDS INCOMPATIBILITY / NEW FEATURES / BUG FIXES]

* subsystem: Descriptive message [GH-1234]
```
For provider development typically the "subsystem" is the resource or data source affected e.g. `resource/vpc`, or `provider` if the change affects whole provider (e.g. authentication logic).

#### Entry Ordering

To order entries, these basic rules should be followed:

1. If large cross-cutting changes are present, we list them as "provider: "
2. Order entries lexicographically in the subsystem (`resource/eks` < `resource/rds`)

#### Example Changelog
```
## 1.27.0 (Unreleased)

BREAKING CHANGES:

  * The only thing that's changed is everything [GH-1]

FEATURES:

  * New resource: eks [GH-43]

IMPROVEMENTS:

  * resource/elb: Super improved [GH-12]
  * resource/vpc: Now better [GH-22, GH-32]
```
The upcoming release is always at the top of the file and is marked specifically as "Unreleased". Each bullet also references the corresponding pull request number that contained the code changes. During the release process the "Unreleased" header is removed and all pull request references are linked to automatically.

#### Versioning

Provider development follows [semantic versioning guidelines](https://semver.org/). In summary, this means that with a version number of the form MAJOR.MINOR.PATCH, the following meanings apply:

- Increasing only the patch number suggests that the release includes only bug fixes, and is intended to be functionally equivalent.
- Increasing the minor number suggests that new features have been added but that existing functionality remains broadly compatible.
- Increasing the major number indicates that significant breaking changes have been made, and thus extra care or attention is required during an upgrade.

For more details please visit a [blog post](https://www.hashicorp.com/blog/hashicorp-terraform-provider-versioning) made regarding Provider versioning.

### Release process

Release cadence will vary project to project. There is no explicit timeline for provider releases, we tend to evaluate each provider release based on a few factors such as the collection of small bug fixes and features or conversely one critical fix or highly demanded feature. Releases can be requested in our slack channel. Smaller providers typically have no consistent cadence, larger ones such as AWS have leaned towards weekly releases. We feel this is probably the most frequent a project should cut releases.

Our release process is automated. That process tags, cross compiles, signs, and uploads the binaries to our [release site](https://releases.hashicorp.com/), the list of our official providers can be found on [terraform.io](https://www.terraform.io/docs/providers/index.html). HashiCorp handles all releases internally, and requests for provider releases can be made in the #committers-terraform Slack channel by mentioning @provider-releases. The employee cutting the release will notify maintainers via Slack before and after release so pull request merging can be avoided during that interval. We compile binaries for all platforms & architectures supported by  [Terraform core](https://www.terraform.io/downloads.html) (currently MacOS, FreeBSD, Linux, OpenBSD, Solaris, Windows). Exceptions can be made if there’s a strong reason a provider cannot be compiled for any of these. Such cases have to be discussed with a HashiCorp employee.
