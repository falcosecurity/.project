# Falco `.project`

This repository holds Falco's canonical YAML metadata. CNCF automation reads the
root files; Falco's additional configuration lives under `config/`.

| File | Source and editable fields |
| --- | --- |
| `project.yaml` | CNCF project metadata; maintainers can edit it |
| `maintainers.yaml` | Generated from active repository `OWNERS`; do not edit. Only `core-maintainers` is `managed: true`; repository rosters contain approvers only |
| `config/repository.yaml` | Edit repository name, scope, and status. Archived repositories are removed automatically |
| `config/org.yaml` | Authoritative Peribolos configuration. Edit admins, organization/repository settings, descriptions, team settings/permissions, and purpose-specific teams. Membership and repository/core team roles are generated; requested emeritus-only departures may remove a member |
| `config/affiliations.yaml` | Edit names, companies, and verified historical aliases (`github_id`, `emeritus_aliases`). Active-repository OWNERS references and admins receive placeholders; obsolete entries are removed |
| `OWNERS` | Root approvers follow the accepted core-maintainer roster through reviewed PRs |

## Updating metadata

Change roles in the relevant repository's root or nested `OWNERS`. The
[`project-sync` Go tool](https://github.com/falcosecurity/test-infra/tree/master/tools/project-sync)
runs in OCI Prow and opens a PR here as `poiana`. After it merges, the tool proposes
updated documentation in `evolution` and root ownership in Special repositories.
Repository catalog edits also update organization configuration and Prow triggers;
affiliation and description edits update documentation. Peribolos applies the
accepted `config/org.yaml` to GitHub.

Active approvers and reviewers qualify for organization membership. Emeritus
maintainers retain membership only when already present in both GitHub and the
accepted `config/org.yaml`; absent emeritus-only accounts are not invited.
Former reviewers have no emeritus retention exception. Manually configured
organization admins remain members. Archived repositories contribute no roles.

To honor a requested departure after all active roles have ended, remove the
emeritus-only member from `config/org.yaml` through a reviewed PR. Preserve the
historical OWNERS entry. Record out-of-band departures before Peribolos next runs,
since it applies the accepted YAML independently.

Verified historical renames can be recorded with `github_id` and `emeritus_aliases`
in `config/affiliations.yaml`. The immutable ID must match the existing member.
These aliases affect emeritus retention only and grant no active review permissions.
Invalid active accounts stop reconciliation; unusable historical emeritus accounts
are omitted and reported. Incomplete membership or account reads stop the run.

Repository teams include approvers and reviewers;
GitHub team maintainers are eligible organization admins, with everyone else a member.

The validation workflow checks YAML schemas on every PR. It permits temporary
differences between editable inputs and generated files so the automation can
propose the corresponding updates after merge. Check generated PR diffs before
approving them, especially organization membership changes.

See [the metadata process](https://github.com/falcosecurity/evolution/blob/main/METADATA.md)
and [operational instructions](https://github.com/falcosecurity/test-infra/blob/master/docs/github-org-management.md).

## CNCF automation

The existing project and maintainer validators remain enabled. Landscape updates
continue to use `project.yaml`. All Falco-specific configuration stays in `config/`
to avoid collisions with future CNCF root files.

- [CNCF `.project` documentation](https://github.com/cncf/automation/tree/main/utilities/dot-project)
- [Schema reference](https://github.com/cncf/automation/blob/main/utilities/dot-project/SCHEMA.md)
