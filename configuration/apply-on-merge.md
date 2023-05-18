# Apply on Merge

By default, Digger does not run apply on merge. This is a safer way to resolve the [merge-apply dilemma](https://itnext.io/pains-in-terraform-collaboration-249a56b4534e) - run apply manually while the PR is still open, then merge when it succeeds in all target environments. But the tradeoff is that your main falls behind the state of your environments; this is not right from the purist point of view (plan = artifact, apply = deploy the artifact).

You can configure Digger to run apply on merge with the following entries in `digger.yml`

```
workflow_configuration:
      on_pull_request_pushed: [digger plan]
      on_pull_request_closed: [digger unlock]
      on_commit_to_default: [digger apply]
```

The tradeoff then of course shifts to handling flaky applies. You'll need to raise a new PR for every change. But that might be preferable in some cases, for example for highly sensitive parts of production environments that are rarely changed. (eg the "foundation infra" layer with VPC definitions etc).
