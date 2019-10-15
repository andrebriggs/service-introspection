# Using AzDO Service Hooks to Enable Service Introspection

This repository covers an alternative approach to decorating Azure Devops Pipelines with metadata to achieve knowledge of the status of a GitOps flow.

## Spektate
[Spektate](https://github.com/Microsoft/spektate) is a dashboard tool to allows a holistic view of container based applications as they flow from source code to container registries to _high level definition_ repositories, _manifest_ repositories and finally Kubernetes cluster deployment.

The visual respresentation is seen below:
![spektate.png](spektate.png)


## Alternative Method To Retrieve CI/CD Metadata

![service-introspection.png](service-introspection.png)


## Examples of Service Hook Event Payloads
### Build Started

`$ cat example-json/started_build.json | jq '.eventType, .createdDate, .message.text, .resource.run.pipeline.id,.resource.run.name, .resource.run.id'`
```
"ms.vss-pipelines.run-state-changed-event"
"2019-10-02T19:54:00.3307136Z"
"Run 20191002.2 in progress."
7
"20191002.2"
479
```

### Build Ended

`$ cat example-json/completed_build.json | jq '.eventType, .createdDate, .message.text, .resource.definition.id, .resource.buildNumber, .resource.id'`

```
"build.complete"
"2019-10-02T19:57:48.7584186Z"
"Build 20191002.2 succeeded"
7
"20191002.2"
479
```

### Stage State Change

`$ cat example-json/stage_state_change.json | jq '.eventType, .createdDate, .message.text, .resource.pipeline.id,.resource.stage.name, .resource.run.id'`

```
"ms.vss-pipelines.stage-state-changed-event"
"2019-10-02T20:10:39.1376851Z"
"Run 20191002.3 stage build running."
7
"build"
480
```



### Example of Get Build Id API Result JSON

`$ cat example-json/get_build_id_api_result.json | jq '.status, .buildNumber, .result,.startTime,.finishTime'`

```
"completed"
"20191002.4"
"succeeded"
"2019-10-02T21:56:04.844615Z"
"2019-10-02T22:00:04.8462149Z"
```