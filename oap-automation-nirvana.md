## PB Initial thoughts

### project definitions

I'm not familiar with a fair number of terms used - can we briefly define (or link to existing definitions) each of the projects that we're discussing current/nirvana automation/deploy states (including what we are developing in and where we get it from, e.g. xml files from an export of an existing project; what we are generating and where it is going, e.g. proprietary gooddata formatted json we post via the web UI, etc)? 

## branch definitions

I'm not sure what we mean when we say "create a branch," "use existing branch," "merges to master," or "create a PR."

1. Are these currently git or some other types of branches/PRs (what kind)?
2. Are they all the same types of branches/PRs regardless of project type?
3. Are they planned to be git branches/PRs in nirvana state?

[AF] I believe we don't have these answers yet - this is part of what we're trying to figure out. I believe the answer to #3 is yes, ideally, but is it possible?
 
## other definitions

Are "deploy" and "publish" used interchangeably below? If so, let's settle on one; if not, let's specify what they mean in context.
[AF] I tried to stick to deploy below.

## consistency of process/flow/terminology between projects

I understand that these are different/disparate projects that will likely have their own unique needs. But for the purposes of this document and ongoing discussion, can we define some broad(er) categories of the automation/deploy process and organize each project's current and nirvana states based on that? Maybe we have categories or steps like:

* branching/development
* PR workflow
* build/compile/unit-test
* testing (integration testing?) environment
* deploy

-------------------

## ETL development & promotion

| current | nirvana |
| ------- | ---------- |
| create copy of graph (through CloudConnect - import graph from existing into new) to be used for dev from any prodcution project | create a branch which takes "master" and creates a dev graph |
| if any comparison is even done now between a new graph and current production, it is done manually by the developer (in CloudConnect?) | PR compares changes in dev graph to master graph at text level (export to XML, parse tags?). This will need substantial parsing to make it useful.|
| Is this item for auto-schedule of testing? If so, none exists. If this is regarding the schedules & processes (bundles of schedules) which run the graph in the project, are set up manually by developer in Dev/QA projects through Data Integration Console in the GD UI (bi.dataxu.com, must have admin access in project); GD somehow deploys these changes when going to production | Auto-schedule / execution set up |
| no master exists; any merging would be done manually based on previous steps | on ship, merges to master |
| Dev/QA: graph is deployed to dev/qa project by developer via CloudConnect; Production: dev/QA graph is handed off to GD, who somehow releases it to all projects (assume that old graph is over-written, not merged, but not sure)| CloudConnect -> Deploy (replaced by API call) on build to target project - dev/ staging/ production |
| See items above regarding differences between deploying changes to dev/QA vs Production. Dev/QA deploys do not require PbH | PoweredByHelper needs to be replaced as well to deploy to multiple projects |

## LDM development & promotion

| current | nirvana |
| ------- | ---------- |
| create copy of LDM (through CloudConnect - import LDM from existing into new) to be used for dev from any production project | create a branch from master LDM |
| if any comparison is even done now between a new LDM and current production, it is done manually by the developer| PR compares changes from XML representation of LDM to master. |
| no master exists; any merging would be done manually based on the previous step | on ship merge to master |
| Dev/QA: LDM is deployed to dev/qa project by developer via CloudConnect; Production: dev/QA LDM is handed off to GD, who somehow releases it to all projects (assume old LDM is over-written, not merged, but not sure)| deploy to dev/ staging / production project |
| set fact grain manually through CloudConnect | API calls to set fact grain in project / grey pages |

## Project development & promotion

| current | nirvana |
| ------- | ---------- |
| create blank dev/qa project from "template" project through GD grey pages, must have admin access to both projects in order to do this | create a branch from one of the masters (PGMasterTemplate, DXManagementTemplate) |
| list of changed/new objects made by dev/qa during development; updated and tracked manually (usually in an Excel spreadsheet or on the DX wiki)| PR creates list of all objects which have changed.. any way to tell what specifically changed within environment?? |
| manually do partial (MD) export from dev project; manually do partial (MD) import into qa project via GD grey pages, must have admin access to both projects; if any errors occur, no good error messaging from GD, developer/qa investigates | on ship all of those objects are put into (export/import?) appropriate master template |
| new dev/qa environment is manually set up by dev/qa (above steps + setting up of ETL and loading historical data, mostly through GD grey pages, but some work done in GD UI (bi.dataxu.com))| Build deploys branch to appropriate environment |


## Report / Dashboard / Metric / etc development & Promotion

| current | nirvana |
| ------- | ---------- |
| dev/qa project set up manually by dev/qa - see "Project development & promotion" above | uses branch from project development |
| no PR; manual QA only - new functionality & regression in GD UI (bi.dataxu.com) | PR at the project level |
| must rely on JIRA tickets for each change, usually these are tracked under one epic, but it is difficult to see in one place all the changes made in even a single project| would like to know at each of these levels, what was the code change (formula, fields used, new/deleted, etc) |