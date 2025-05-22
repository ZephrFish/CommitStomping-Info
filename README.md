# TXXXX (ATT&CK ID TBC): Commit Stomping

**ID:** TXXXX  (ATT&CK ID TBC) 
**Tactic:** Defence Evasion, Impact   
**Platform:** Git (local and remote), GitHub, GitLab, Bitbucket, Gitea  
**Permissions Required:** Commit access to a Git repository  
**Effective Permissions:** Author or committer rights  
**Data Sources:** Version control logs, CI/CD system records, commit signature validation, code review activity  
**Contributors:** Andy Gill (ZephrFish)  
**Version:** 1.0  
**Created:** 15 May 2025  



## Description

Commit Stomping is a technique in which Git commit timestamps are manipulated to obscure the true timing of changes. Drawing from concepts used in file timestomping, this technique allows an attacker or insider to mislead code reviewers, incident responders, and auditors by altering the perceived chronology of commits in a repository.

Git stores two primary time values with each commit:

- **GIT_AUTHOR_DATE**: when the content was originally written  
- **GIT_COMMITTER_DATE**: when it was committed to the repository

Both can be set or overridden using environment variables or history rewriting tools like `git rebase` or `git filter-branch`. This manipulation is not a vulnerability in Git—it is part of its design—but it can be used maliciously to cover tracks, mask unauthorised changes, or insert code into past points of a project’s timeline.

Commit Stomping can undermine trust in version control data and weaken software supply chain integrity in organisations where commit history is used for compliance checks, attribution, or as part of a secure development pipeline.


## Procedure Examples

- A developer alters commit dates to make a recent backdoor appear as part of an earlier release.  
- During an investigation, a malicious user rewrites timestamps to mislead forensic timelines and obscure when credentials or logic were added.  
- An attacker automates timestamp randomisation across a codebase to inject entropy into the commit log and derail efforts to reconstruct a sequence of events.

## Detection

Commit Stomping can be identified through a combination of metadata analysis and comparison with trusted external records:

- Look for identical or highly similar timestamps across multiple commits from the same user.  
- Flag large discrepancies between `GIT_AUTHOR_DATE` and `GIT_COMMITTER_DATE`.  
- Check for commits that are out of chronological sequence compared to their position in the log.  
- To identify inconsistencies, compare commit metadata with CI/CD pipeline logs, mirrored repositories, and webhook data.  
- Monitor for unusual clusters of commits dated in the distant past, particularly when made in bulk or by a single contributor.



## Mitigations

- **Enforce Signed Commits**: Require GPG or SSH-signed commits to establish trust in who authored and committed the code. While this does not prevent timestamp manipulation, it adds accountability.  
- **Capture External Metadata**: Record when a commit was received by CI/CD systems, internal Git servers, or code review tools to provide a trusted reference point.  
- **Use Mirrored Repositories**: Maintain read-only mirrors to detect tampering or unauthorised rewriting of history.  
- **Disable Force Pushes**: Prevent history rewriting on protected or main branches to avoid unauthorised alterations.  
- **Monitor for Anomalies**: Use commit analysis tools or internal scripts to flag suspicious timestamp patterns and unusual contributor behaviour.


## References

- Gill, A. “Commit Stomping – Manipulating Git Timestamps.” [blog.zsec.uk/commit-stomping](https://blog.zsec.uk/commit-stomping)  
- Git Book – Environment Variables: [https://git-scm.com/book/en/v2/Git-Internals-Environment-Variables](https://git-scm.com/book/en/v2/Git-Internals-Environment-Variables)  
- `git-filter-repo` (preferred over `filter-branch`): [https://github.com/newren/git-filter-repo](https://github.com/newren/git-filter-repo)



## Related Techniques

| ID         | Name                             | Description                                                                 |
|---|-|--|
| T1070.006  | Timestomp                        | Alters file timestamps to evade forensic investigation.                     |
|T1565.001  | Stored Data Manipulation: Source | Modification of source code or development artefacts.                      |
| T1609      | Container Administration Command | May relate where Git timelines feed into DevOps or containerised pipelines. |

