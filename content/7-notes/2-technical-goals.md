# Goal

This is a significant undertaking which aims to wrap git below simpler save/load semantics. This also led to a major refactoring of `docsmith` to enable this abstraction.

---

## Affordances

The end goal is to have the following semantics:

```
apprentice init                                        // Setup workspace
                                                       // ~~Install~~ Clone and install content packages.

apprentice new                                         // Prompts for name of new project and template
                                                       // Does `apprentice new ${PROJECT} ${TEMPLATE}`
                                                       // Confirm.

apprentice new iilab/my-course                         // Check if configure. Creates .content and clone, and checks out tag.
                                                       // Uses default template (clones depth 1)
                                                       // Creates and checks out new personal work branch
                                                       // Ask to make default
                                                       // Ask to start

apprentice save                                        // Check if npm owner. If yes, ask if save template or save new project.

```

> Loading should be doable both for published packages (on npm) and git based repos.
>   - Load local package: `activist-apprentice-course-template`
>   - If not found, load npm: `activist-apprentice-course-template`
>   - Load git: `https://github.com/smallworldnews/activist-apprentice-course-template.git`

```
apprentice load activist-apprentice-course-template    // `configure`: Check if configure. Creates .content and clone, and checks out tag.
                                                       // `store/create`: Creates and checks out new personal work branch
                                                       // `package/check`: Checks npm for new release.
                                                       // `store/update`: Asks for update (rebase).
                                                       // `configure/default`: Ask to make default

apprentice save activist-apprentice-course-template                  // `store/commit`: Commits to personal work branch
                                                       // `package/check`: Checks npm for new release.
                                                       // `store/update`: Asks for update (rebase).
                                                       // `sync`: Pushes branch
                                                       // `submit/info`: Give instruction about submitting for review.
                                                       // `configure/default`: Ask to make default

apprentice submit                                      // Checks npm for new release. Asks for update.
                                                       //
                                                       //

apprentice start                                       // Checks npm for new release. Asks for update.
                                                       //

apprentice publish                                     // Checks for role on npm
                                                       //                                         
```

---

## Architecture

The beta branch (future version 0.7) of docsmith aims at separating concerns in the following way:

 - User commands
   - `new`
   - `load`
   - `save`
   - `start` / `preview`
   - `submit`
   - `publish`
 - `internal`
   - `edit`: wrapping filesystem (maybe atom)
   - `store`: wrapping git
   - `review`: wrapping github/gitlab PR/MR
   - `package`: wrapping npm
   - `render/web`: wrapping metalsmith
   - `render/mobile`: wrapping expo
 - pre/post/check -> middleware/hof
   - `configure`: check that workspace is configured.
   - `update`: checks that workspace and content packages are up to date
   - `sync`: persist info in .content, detect if online, queue actions when offline, execute queue when online.

<details>
  <summary>Implementation Notes - Click to expand</summary>
<pre><code>
Maybe separate concerns between:
 - `interface`
   - `new`
   - `load`
   - `save`
   - `start` / `preview`
   - `submit`
   - `publish`
 - `internal`
   - `edit`: wrapping filesystem (maybe atom)
   - `store`: wrapping git
   - `review`: wrapping github/gitlab PR/MR
   - `package`: wrapping npm
   - `render/web`: wrapping metalsmith
   - `render/mobile`: wrapping expo
 - pre/post/check -> middleware/hof
   - `configure`: check that workspace is configured. `load`/
   - `update`: checks that workspace and content packages are up to date
   - `sync`: persist info in .content, detect if online, queue actions when offline, execute queue when online.

#### configure

Default to creating global ~/.content and link it to current workspace. (Using this as a workspace detection feature).
Register workspace in `.content/content.yml` file.
Allow multi configuration (for development, isolation) by reading the relative `.content` folder.

#### Testing

What should be a testing strategy to make the git wrapping more robust? It feels that at some point the git wrapping should go in its own module.
  - I should probably have fixtures both for:
    - The git layer. As bare repos?
      - I should be able to use nodegit to make the repo fixtures declarative.
    - The github/gitlab layer. As API mocks?

  - Check corner cases:
    - force pushes (allow in branches but fail with instructions on master)
    - tag mutation (warn?)

#### Overall

Workspace is for content folder access and manipulation.
`.content` should have the git checkout instead of the module. ~~Possibly there could be a read only mode where we use the module. (let's not to keep things simple)~~

Assumptions:
 - Gitlab? Github?
 - Personal forks are not shared. (so rebase to main repo are ok)

init should:
 - check for existing access to git user info.
   - if not, display instructions.
 - check for existing user fork for content packages.
   - if not, display instructions and URL to create fork.
 - clone the fork (declared in the content.yml of the CLI tool).
   - Checkout tag (i.e. published version), not master.

start/update should check for changes and:
 - no changes: `git pull`
 - check changes on startup.
   - fetch master and tags, new tag?
     - if new tag, try to rebase new tag onto auto-save branch.
       - deal with conflicts.
       - if succeed create new autosave branch with new tag version?
     - if new commits on master, try to fast forward master onto auto-save branch

 - auto-save
   - `metalsmith-contentascode-autosave`
     - Auto-commit can be annoying because of lack of messages and control over granularity. (squashing on save should address this)
     - Editor auto-saving in the editor should be enabled. (i.e. autofile save, irrespective of auto-commit)
     - Auto-commit in a personal autosave branch and push on exit (and/or every 5 minutes) might be a good safety feature.
     - Then squashing auto-commits from the autosave branch
     - save to branch (v0.x.x-auto-y)

Autosave branch switches from a named-branch:
 - When new "save as" to new named branch
 - After load of other named branch or of version tag.

When is increment useful? When reloading tag and starting to work which avoids loosing autosaves from previous session. So increment should occur on "save as" and "load".

Model:
 - Current tag. (tip of minor release? what about when merge is happening?)
 - Current autosave branch (v0.x.x-auto-y or v0.x.x-named-save-y).

#### No fork.

 - init/update:
   - check if new tags:
     - If yes, then this is a major/minor update.
   - check if release branch has advanced. (Can we avoid this on the first version?)
     - If yes, then this is a patch update.
   - If both, then do only major/minor update, but if it fails try patch update.
   - If major/minor update (from v0.a to v0.b):
     - Double check with the user what the current local version is. Show all autosave branches? only the most recent? only the "current" one?
     - Create new incremented local version from current local version (v0-a-my-save-321) with new version as base branch name using tag name conversion convention (v0-b-my-save-322)
     - Rebase new release branch (upstream v0-b) onto renamed current local version (v0-b-my-save-322)
     - Deal with conflicts
     - Switch to updated (rebased, renamed, incremented) v0-b-my-save-321
   - If patch update (from v0.a.x to v0.a.y)
     - Double check with the user what the current local version is. Show all autosave branches? only the most recent? only the "current" one?
     - Create new incremented local version from current local version (v0-a-my-save-321) i.e. (v0-a-my-save-322)
     - Rebase fetched current release branch (upstream v0-a) onto current local version (v0-a-my-save-322)
     - Deal with conflicts
     - Switch to updated (rebased, incremented) v0-a-my-save-322

  - save:
    - Best UI would be to trigger this from an atom pane web preview, a browser based web preview. (display number of changes in badge on "Save" button). Save would then open the Git tab and focus on the commit message box in Atom. Or, the web preview would open a message box, and confirming would send a payload via websocket to a metalsmith plugin.
    - ??? fetch upstream to pre-empt stale PR ?
    - Enter commit message.
    - Attempt to push to remote fork.
    - Ask if want to **submit** change.
      - If yes, then offer instructions.

#### No Fork / No release branch

Assumption:
 - Master is protected to make branch reviews mandatory.

 - init
   - Check for git config
   - `git clone $repo`
   - `git checkout -b user/$username`

 - update
   - `git fetch`
   - `git checkout user/$username`
   - `git merge master`
   - Deal with conflicts
 - save
   - Always `update` first
   - `git push master`


#### Deal with conflicts

 - Detect `error: could not apply fa39187` somehow.
 - Open Atom in Merge Conflict mode.
 - Continue (or Abort).
   - `git rebase --continue`

### Broader questions

Do we need release branches (usually needed for non-SaaS distributed software packages) because of the possible content dependency network? With them it might allow:
 - To push hotfixes more easily by having dependents merge the tip of the branch.

This really depends on whether:
 - everything is doable just with tags? (Seems that not).
 - there is a strategy for obsolescense of versions. (ping back with transclusion client?)
 - how this mixes with npm publishing (should be fine).

Ping back with transclusion client if it includes fork name would allow to alert authors of conflicting changes or people working on same content before merge.

Should this wrap npm under these simplidied git semantics?
</code></pre>
</details>


## Status

 - [ ] Refactor `content.yml` settings (breaking API change) to:
   - Simplify multi-instance configuration
   - Enable `templates` too bootstrap `new` projects.
 - [ ] Refactor to enable async git and npm, error reporting in an elegant way
 - [ ] `apprentice init` git clone, npm install and sets up workspace.
 - [ ] `apprentice load` (auto run on start)
 - [ ] `apprentice save`
 - [ ] `apprentice new`

 ---
