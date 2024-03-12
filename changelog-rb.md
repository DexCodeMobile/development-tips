# Changelog - Tools

On the iOS side, we use the [changelog-rb](https://github.com/pinglamb/changelog-rb) tool to maintain a changelog. This should be maintained during development and release as features are added/removed, etc.

For more information on changelog style, see [keepachangelog.com](keepachangelog.com)

For more information on the changelog-rb tool, see its Github repo. Alternatively, type `changelog help` in your terminal.



## Example
```
// Add changelog. It created a unreleased folder.
changelog add "NSV-1234: JIRA Ticket title or a proper and simple description" -t Added
 
 
// Add tag for a release version. It moves all of logs from unreleased folder to the release version folder.(i.e. /3.0.19)
changelog tag 3.0.19
 
 
// Update CHANGELOG.md file. It prints all of logs for the release version.
changelog print
```


