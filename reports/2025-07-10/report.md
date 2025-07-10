# Chef OSS Standardization Report

## First Decommissioning Batch

### Where the Batch Comes From - Internal Review Board

An internal review board is meeting weekly to initially categorize repos, using filtered lists based on the metrics described in the previous report.

Operations, Product, DevRel, Engineering Management, Architecture are represented on the board. The board currently intakes 20 repos per week. 

The job of the internal review board is to assign each repo to one of six **workflows** (see below). At this time, only the Decommission and Existing-Boneyard-Archive workflows are being implemented; Existing-Boneyard-Archive does not require community interaction. This section describes the repos selected for Decommisioning; for details of the Decommisioning process, see the Workflows section. Note that the first step of the Decommissioning process is a 30-day feedback period.

### How to Provide Feedback for a Decommission Decision

Each repo selected for decommissioning will have a GitHub Issue opened with the title "This repo ORG/REPO is being considered for archival" and the label `oss-standards`.

To comment on the proposed decommissioning, simply comment on the issue.

To indicate that you wish the repo to NOT be decommissioned, react with a "thumbs down" to the issue (and please be sure to comment).

If you cannot or do not wish to comment on a GitHub Issue, you may use the [alternate form](https://forms.office.com/r/92C2BJ8iJs).

Not all requests can be honored. Some requests may be better served by forking the repo.

### Batch selection criteria

This batch was selected from repos that matched these criteria: 
  * public, unarchived
  * no known dependencies on Chef products
  * not an Erlang or Rust project (our dep detector is currently unreliable on these dep types)
  * not in chef-boneyard or in habitat-sh (excluded due to Rust)

We sorted based on usage_metric first, then enagement_metric, considering the least-used, least-maintained repos first.  These metrics were defined in the 2025-06-12 report, as was the dependency analysis process.

### List of Repos Proposed for Decommissioning

This is the batch for 2025-07-10. This list is available as a CSV [here](decommission-notice-issues.csv).

Links are to the discussion issue for the repo.

 1. [chef/jazz_hands](https://github.com/chef/jazz_hands/issues/1)
 1. [chef/go-bindata](https://github.com/chef/go-bindata/issues/2)
 1. [chef/ruby-unf_ext](https://github.com/chef/ruby-unf_ext/issues/1)
 1. [chef/nginx](https://github.com/chef/nginx/issues/1)
 1. [chef/oauth-ng](https://github.com/chef/oauth-ng/issues/1)
 1. [chef-cookbooks/chef_zpool](https://github.com/chef-cookbooks/chef_zpool/issues/5)
 1. [chef/testing-4-cloud](https://github.com/chef/testing-4-cloud/issues/1)
 1. [chef/dafne-online-habitat](https://github.com/chef/dafne-online-habitat/issues/1)
 1. [chef/docker-buildkite-plugin](https://github.com/chef/docker-buildkite-plugin/issues/1)
 1. [chef/openvpn_cfssl](https://github.com/chef/openvpn_cfssl/issues/1)
 1. [inspec/input-file-exists-profile](https://github.com/inspec/input-file-exists-profile/issues/1)
 1. [inspec/inspec-test-profile-chef-node-input](https://github.com/inspec/inspec-test-profile-chef-node-input/issues/1)
 1. [chef/foulkon](https://github.com/chef/foulkon/issues/6)
 1. [chef-cookbooks/now](https://github.com/chef-cookbooks/now/issues/1)
 1. [inspec/inspec-test-profile-run-context-audit](https://github.com/inspec/inspec-test-profile-run-context-audit/issues/1)
 1. [inspec/inspec-integration-profile](https://github.com/inspec/inspec-integration-profile/issues/1)


## Workflow Updates

The previously identified workflows have been further developed. 

Previously, we described the Standardize and Decommission major workflows, but have identified several more minor workflows. All planned workflows are described below.

Each workflow represents the fate of a repo being processed. Each has a description of the workflow as well as a readout of the proposed configuration of the workflow automation file, which determines the proposed steps to be manually or automatically executed.  We'll cover details of what the steps entail in future reports; also please note that these lists are currently in development and may change.

### The biggest changes: deletion and privatization

As the internal review process has progressed, it has been clear that more aggressive outcomes are needed than simply "Archive/Keep/Fork." These outcomes are intended to be used sparingly.

#### We need to be able to delete (rarely)

We have discovered several completely empty (unitialized) repos, self-development repos, and that sort of thing. Decommissioning takes time and effort that are not worth the investment.  We are giving ourselves permission to delete selected repos:

 * with a public notice and feedback period
 * with a contents-restorable backup method (including .git directory)

While some repos are currently being considered internally for deletion, the workflow for deletion is under development. No actions toward deletion of a public repo will be taken without community feedback.

The community feedback process will be similar to that for decommissioning repos, with a perhaps longer feedback window.

#### We need to be able to take repos private (rarely)

This is a process of discovery, and we may uncover data that we do not need, wish, or be able to remain public, due to policy or complicance requirements. Keep in mind that as a publicly traded and ITIL-compliant company, the Chef Division of Progress Software Corporation is under tighter constraints than Chef Software (the prior entity) was. Progress reserves the right to privatize a repo for any or no reason when permitted.

This option is only available when practical and compatible with the license in the repo, if any.

Unfortunately, Progress is unable to provide community feedback or notice for this action at this time.

For more information or concerns, please join the regularly scheduled Community Advisory Council meetings.

### Workflow Details

#### decommission

##### Description

Declare a repo no longer maintained, but keep it available for reference. Modify messaging, move to chef-boneyard, and archive the repo.

##### Examples

Disused cookbooks, obsolete libraries, sunsetted products.

##### Steps

```yaml
    actions:
      - action: wait_for_public_feedback
      - action: disable_ci
      - action: disable_dependabot
      - action: add_note_and_link_to_readme
      - action: replace_contributors_archive
      - action: license_check
      - action: copyright_update
      - action: spurious_file_check
      - action: set_description_to_old_url
      - action: finalize_decommission
```

#### delete

##### Description

Decide the data will never need to be accessed again except in an emergency. Notify the public, take a backup, and permanently delete the repo. To be used carefully, as this breaks any consumers and removes the repo from public availability. 

##### Examples

Self-development repos, test fixture data for obsolete products, stale forks without known downstrem contributions

##### Steps

```yaml
    actions:
      - action: wait_for_public_feedback
      - action: backup_clone_of_repo
      - action: delete_repo
```

#### existing-boneyard-archive

##### Description

There are many repos in chef-boneyard that are not in the 'archived' state. This workflow, with as little ceremony as possible, will archive the repo.

No community feedback period is provided.

##### Examples

Any public, non-archived repo in chef-boneyard.

##### Steps

```yaml
    actions:
      - action: disable_ci
      - action: disable_dependabot
      - action: add_note_and_link_to_readme
      - action: replace_contributors_archive
      - action: license_check
      - action: copyright_update
      - action: finalize_decommission
```

#### make-private

##### Description

Make the repository private but keep it active. A carefully considered and rarely used option, to be used when Progress has no obligation to keep the code public, and some obligation or other motivation to no longer expose the contents to the world.

No community notice or feedback period will be given.

##### Examples

Proprietary customer demos; repos containing legal, financial, or customer data; security vulnerability information that violates current disclosure policy; other categories to which Progress policy may apply.

##### Steps

This workflow is being developed, and the workflow may remain propietary.

#### standardize

##### Description

This is the workflow intended for actively maintained repos thaat have a future at Progress Chef. This workflow brings the repo into alignment with current policies and practices, ensures access controls are proper, and sets up a framework for ongoing compliance.

##### Examples

All flagship product repos (chef/chef, inspec/inspec, etc), all active dependent libraries (mixlib-*, for example), CI support repos - anything in use.

##### Steps

```yaml
    actions:
      - action: determine_release_branches
      - action: verify_dependabot_enabled
      - action: replace_contributors_active
      - action: remove_sla_from_readme
      - action: license_check
      - action: copyright_update
      - action: redirect_security_file
      - action: redirect_code_of_conduct
      - action: spurious_file_check
      - action: enforce_branch_protection_rules
      - action: scrub_codeowners
      - action: check_readme_for_governance
```

#### stale-fork-archive

##### Description

Chef often needs to make a modification to a 3rd-party open-source project, often a bugfix or a customization. The proper workflow is to fork (make a copy) of the 3rd-party repo into the `chef` org, make the change in teh "downstream" copy, and "push" the change to the "upstream" original project. The updtream often does not accept the change, leaving the downstream copy as the only extant copy of the Chef effort. In addition, this process leaves many downstream copies laying around after the one-time change is in place.

##### Examples

`chef/ruby` is a large example, but there are many.

##### Steps

```yaml
    actions:
      - action: disable_ci
      - action: disable_dependabot
      - action: detect_stale_fork_contributions
      - action: add_fork_note_to_readme
      - action: add_fork_contributions_to_readme
      - action: finalize_decommission
```

#### private-archive

##### Description

Similar to `make-private`, this workflow takes the repo private, but also decommissions the repo, taking it out of active service. A carefully considered and rarely used option, to be used when Progress has no obligation to keep the code public, and some obligation or other motivation to no longer expose the contents to the world.

No community notice or feedback period will be given.

##### Examples

Proprietary customer demos; repos containing legal, financial, or customer data; security vulnerability information that violates current disclosure policy; other categories to which Progress policy may apply.

##### Steps

This workflow is being developed, and the workflow may remain propietary.


## Staff Updates

Three archive staffers have joined the Chef team for the purposes of this initiative, and started their engagement with Progress Chef on Monday July 7. They will be engaged in data gathering, manual remediation, and implemtation of automation.
