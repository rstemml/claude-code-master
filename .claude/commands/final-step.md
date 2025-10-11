# Run Post Steps

Run these steps to finalize the release of a new feature, bug or chore.

1. Get the current version in the package.json in the main branch
2. Increase the version number in the package.json of beta-chat directory.

use semantic versioning

<link>
https://semver.org/
</link>

2. If it is relevant feature for the user, use the scratchpad or the prd for the feature to write customer friendly release note.
   - get the current branch and check the history of the changes and check the prd
   - add this release note to the feature-log-data to the top of the list.
   - important: only add a note if this relevant for the user
   - never use customer specific phrases
