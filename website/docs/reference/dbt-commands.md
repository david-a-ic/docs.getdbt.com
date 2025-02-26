---
title: "dbt Command reference"
---

You can run dbt using the following tools:

- In your browser with the [dbt Cloud IDE](/docs/cloud/dbt-cloud-ide/develop-in-the-cloud) 
- On the command line interface using the [dbt Cloud CLI](/docs/cloud/cloud-cli-installation) or open-source [dbt Core](/docs/core/installation-overview).
  
A key distinction with the tools mentioned, is that dbt Cloud CLI and IDE are designed to support safe parallel execution of dbt commands, leveraging dbt Cloud's infrastructure and its comprehensive [features](/docs/cloud/about-cloud/dbt-cloud-features). In contrast, `dbt-core` _doesn't support_ safe parallel execution for multiple invocations in the same process. Learn more in the [parallel execution](#parallel-execution) section.

## Parallel execution

dbt Cloud allows for parallel execution of commands, enhancing efficiency without compromising data integrity. This enables you to run multiple commands at the same time, however it's important to understand which commands can be run in parallel and which can't.

In contrast, [`dbt-core` _doesn't_ support](/reference/programmatic-invocations#parallel-execution-not-supported) safe parallel execution for multiple invocations in the same process, and requires users to manage concurrency manually to ensure data integrity and system stability.

To ensure your dbt workflows are both efficient and safe, you can run different types of dbt commands at the same time (in parallel) &mdash; for example, `dbt build` (write operation) can safely run alongside `dbt parse` (read operation) at the same time. However, you can't run `dbt build` and `dbt run` (both write operations) at the same time.

dbt commands can be `read` or `write` commands:

| Command type | Description | <div style={{width:'200px'}}>Example</div> |
|------|-------------|---------|
| **Write** | These commands perform actions that change data or metadata in your data platform.<br /><br /> Limited to one invocation at any given time, which prevents any potential conflicts, such as overwriting the same table in your data platform at the same time. | `dbt build`<br />`dbt run` |
| **Read** | These commands involve operations that fetch or read data without making any changes to your data platform.<br /><br /> Can have multiple invocations in parallel and aren't limited to one invocation at any given time. This means read commands can run in parallel with other read commands and a single write command.| `dbt parse`<br />`dbt compile`|

## Available commands

<VersionBlock firstVersion="1.6">

The following sections outline the commands supported by dbt and their relevant flags. They are available in all tools and all [supported versions](/docs/dbt-versions/core) unless noted otherwise. You can run these commands in your specific tool by prefixing them with `dbt` &mdash; for example, to run the `test` command, type `dbt test`.

For information about selecting models on the command line, refer to [Model selection syntax](/reference/node-selection/syntax).

Commands marked with a '❌' indicate write commands and those with a checkmark ('✅') indicate read commands.

| Command | Description | Parallel execution | <div style={{width:'250px'}}>Caveats</div> |
|---------|-------------| :-----------------:| ------------------------------------------ |
| [build](/reference/commands/build) | Build and test all selected resources (models, seeds, snapshots, tests) |  ❌ | All tools <br /> All [supported versions](/docs/dbt-versions/core) | 
| cancel | Cancels the most recent invocation. | N/A | dbt Cloud CLI <br /> Requires [dbt v1.6 or higher](/docs/dbt-versions/core) |
| [clean](/reference/commands/clean) | Deletes artifacts present in the dbt project |  ✅ | All tools <br /> All [supported versions](/docs/dbt-versions/core) |
| [clone](/reference/commands/clone) | Clone selected models from the specified state |  ❌ | All tools <br /> Requires [dbt v1.6 or higher](/docs/dbt-versions/core) |
| [compile](/reference/commands/compile) | Compiles (but does not run) the models in a project |  ✅ | All tools <br /> All [supported versions](/docs/dbt-versions/core) |
| [debug](/reference/commands/debug) | Debugs dbt connections and projects | ✅ | dbt Cloud IDE, dbt Core <br /> All [supported versions](/docs/dbt-versions/core) |
| [deps](/reference/commands/deps) | Downloads dependencies for a project |  ✅ |  All tools <br /> All [supported versions](/docs/dbt-versions/core) |
| [docs](/reference/commands/cmd-docs) | Generates documentation for a project |   ✅ | All tools <br /> All [supported versions](/docs/dbt-versions/core) |
| [environment](/reference/commands/dbt-environment) | Enables you to interact with your dbt Cloud environment. |   N/A | dbt Cloud CLI <br /> Requires [dbt v1.5 or higher](/docs/dbt-versions/core) |
| help | Displays help information for any command | N/A | dbt Core, dbt Cloud CLI <br /> All [supported versions](/docs/dbt-versions/core) |
| [init](/reference/commands/init) | Initializes a new dbt project |   ✅ | dbt Core<br /> All [supported versions](/docs/dbt-versions/core) |
| [list](/reference/commands/list) | Lists resources defined in a dbt project |  ✅ | All tools <br /> All [supported versions](/docs/dbt-versions/core) |
| [parse](/reference/commands/parse) | Parses a project and writes detailed timing info |  ✅ | All tools <br /> All [supported versions](/docs/dbt-versions/core) |
| reattach | Reattaches to the most recent invocation to retrieve logs and artifacts. |   N/A | dbt Cloud CLI <br /> Requires [dbt v1.6 or higher](/docs/dbt-versions/core) |
| [retry](/reference/commands/retry) | Retry the last run `dbt` command from the point of failure |  ❌ | All tools <br /> Requires [dbt v1.6 or higher](/docs/dbt-versions/core) |
| [run](/reference/commands/run) | Runs the models in a project |   ❌ | All tools <br /> All [supported versions](/docs/dbt-versions/core) |
| [run-operation](/reference/commands/run-operation) | Invoke a macro, including running arbitrary maintenance SQL against the database | ❌ | All tools <br /> All [supported versions](/docs/dbt-versions/core) |
| [seed](/reference/commands/seed) | Loads CSV files into the database |  ❌ | All tools <br /> All [supported versions](/docs/dbt-versions/core) |
| [show](/reference/commands/show) | Preview table rows post-transformation | ✅ |  All tools <br /> All [supported versions](/docs/dbt-versions/core) |
| [snapshot](/reference/commands/snapshot) | Executes "snapshot" jobs defined in a project |  ❌ | All tools <br /> All [supported versions](/docs/dbt-versions/core) |
| [source](/reference/commands/source) | Provides tools for working with source data (including validating that sources are "fresh") | ✅ | All tools<br /> All [supported versions](/docs/dbt-versions/core) |
| [test](/reference/commands/test) | Executes tests defined in a project  |  ✅ | All tools <br /> All [supported versions](/docs/dbt-versions/core) |
| [--version](/reference/commands/version) | Displays the currently installed version of dbt CLI |  N/A | dbt Core, dbt Cloud CLI  <br />  All [supported versions](/docs/dbt-versions/core) | <br />

Note that some have "N/A" since they're not relevant to the parallelization of dbt commands.
</VersionBlock>

<VersionBlock lastVersion="1.5">

Select the tabs that are relevant to your development workflow. For example, if you develop in the dbt Cloud IDE, select **dbt Cloud**.  

<Tabs>
<TabItem value="cloud" label="dbt Cloud IDE">

Use the following dbt commands in the [dbt Cloud IDE](/docs/cloud/dbt-cloud-ide/develop-in-the-cloud) and use the `dbt` prefix. For example, to run the `test` command, type `dbt test`.

- [build](/reference/commands/build): build and test all selected resources (models, seeds, snapshots, tests)
- [clone](/reference/commands/clone): clone selected nodes from the specified state (requires dbt 1.6 or higher)
- [compile](/reference/commands/compile): compiles (but does not run) the models in a project
- [deps](/reference/commands/deps): downloads dependencies for a project
- [docs](/reference/commands/cmd-docs) : generates documentation for a project
- [retry](/reference/commands/retry): retry the last run `dbt` command from the point of failure (requires dbt 1.6 or later)
- [run](/reference/commands/run): runs the models in a project
- [run-operation](/reference/commands/run-operation): invoke a macro, including running arbitrary maintenance SQL against the database
- [seed](/reference/commands/seed): loads CSV files into the database
- [show](/reference/commands/show): preview table rows post-transformation
- [snapshot](/reference/commands/snapshot): executes "snapshot" jobs defined in a project
- [source](/reference/commands/source): provides tools for working with source data (including validating that sources are "fresh")
- [test](/reference/commands/test): executes tests defined in a project

</TabItem>

<TabItem value="cli" label="dbt Core">

Use the following dbt commands in [dbt Core](/docs/core/installation-overview) and use the `dbt` prefix. For example, to run the `test` command, type `dbt test`.

- [build](/reference/commands/build): build and test all selected resources (models, seeds, snapshots, tests)
- [clean](/reference/commands/clean): deletes artifacts present in the dbt project
- [clone](/reference/commands/clone): clone selected models from the specified state (requires dbt 1.6 or higher)
- [compile](/reference/commands/compile): compiles (but does not run) the models in a project
- [debug](/reference/commands/debug): debugs dbt connections and projects
- [deps](/reference/commands/deps): downloads dependencies for a project
- [docs](/reference/commands/cmd-docs) : generates documentation for a project
- [init](/reference/commands/init): initializes a new dbt project
- [list](/reference/commands/list): lists resources defined in a dbt project
- [parse](/reference/commands/parse): parses a project and writes detailed timing info
- [retry](/reference/commands/retry): retry the last run `dbt` command from the point of failure (requires dbt 1.6 or higher)
- [rpc](/reference/commands/rpc): runs an RPC server that clients can submit queries to
- [run](/reference/commands/run): runs the models in a project
- [run-operation](/reference/commands/run-operation): invoke a macro, including running arbitrary maintenance SQL against the database
- [seed](/reference/commands/seed): loads CSV files into the database
- [show](/reference/commands/show): preview table rows post-transformation
- [snapshot](/reference/commands/snapshot): executes "snapshot" jobs defined in a project
- [source](/reference/commands/source): provides tools for working with source data (including validating that sources are "fresh")
- [test](/reference/commands/test): executes tests defined in a project

</TabItem>

</Tabs>
</VersionBlock>
