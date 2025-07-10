# Repository Standarization - Decommissioning

If you have reached this page, you are seeking mor einformation about the Progress Chef decommissioning process. You may have been sent here because you are interested in a repo that has been selected for potential decommissioning, and you want to know what this means for you.

For information about how a repo is selected for decommissioning, see [Categorization](categorization.md).

## What Will Happen

### Workflow Control File: List of Steps

```yaml
    actions:
      - action: wait_for_public_feedback
      - action: disable_ci
      - action: disable_dependabot
      - action: reset_codeowners
      - action: add_note_and_link_to_readme
      - action: replace_contributors_archive
      - action: license_check
      - action: copyright_update
      - action: spurious_file_check
      - action: set_description_to_old_url
      - action: finalize_decommission
```

### Step Details

This section is under development.

#### finalize_decommission

* Close all issues with a comment and label.
* Close all PRs with a comment and preserve branches and label.
* Ensure repo name is unique in chef-boneyard and rename if needed
* Move the repo cross-org to chef-boneyard
* Archive the repo

## Ongoing Support

Progress Chef provides no ongoing support or security updates for any product or library contained in a decommissioned repo. Content in decommissioned repos is generally long-sunsetted and long-unmaintained. No support or migration plan is provided for the decommissioning process.

Users are advised that unmaintained software accumulates security vulnerabilities and platform incompatibilities. 

If you are a Chef Customer requiring ongoing support or updates to a repo selected for decommissioning, this indicates Progress is unaware of your usage, and should be notified so a maintenance or migration plan can be established. Please use the Keep Request process to air your concern ASAP, and read Impact to Ongoing Operations.

## Keep Request Process

A "Keep Request" is a request to keep a repository active - in other words, abort the decommission process. Since this involves considerable investment on Progress's part, not all requests can be honored.

The keep request process is as follows. Each repo being considered for decommissioning will have a GItHub Issue opened, with a fixed title, and instructions on how to proceed. To request the repo be retained:

 * If you are a GitHub user, "react" with a "thumbs-down" to the issue, and make a comment stating your case
 * Otherwise, use the provided link in the issue to file a request using the alternate request form.

Often, Progress may not be the best future maintainer of a project, in which case [forking](forking.md) may be the best outcome.

## Impact to Ongoing Operations

As part of the decommissioning process, several events will occur that will one might suspect will impact operations if the repo is in use.

 * Repo rename (possible, rare). Clones will continue to work with redirection.
 * Org move (all). Clones will continue to work with redirection.
 * Org move (all). Member access rights will drop to new org membership, preventing pushes, but the repo will be read-only anyway.
 * Archive (all). The repo will no longer accept fixes.

## A possible future - Forking

If you wish the repo to be actively maintained, you may consider taking on the maintenance yourself, or finding someoneto do so. This is called a `successor fork`. Progress welcomes such endeavours of decommissioning open-source projects. For details, see [Forking Information](forking.md).

## Staff Contact

Clinton Wolfe
wolfe@progress.com