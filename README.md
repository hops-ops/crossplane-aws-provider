# crossplane-aws-provider-stack

Installs the Crossplane AWS provider family, selected AWS family providers, PodIdentity, and AWS ProviderConfig.

## Why This XR?

Without a dedicated provider XR, provider lifecycle changes are coupled to
the Crossplane core install. With this XR, the provider package, runtime
resources, and ProviderConfig can be reconciled independently.

## The Journey

### Stage 1: Getting Started

Apply the minimal example in `examples/awsproviderstacks/minimal.yaml` after
the target cluster already has Crossplane core installed.

### Stage 2: Growing

Override `spec.runtimeConfig` when the provider controller needs different
requests or limits, and set `spec.nodePool.enabled: true` to land provider
runtimes on the CrossplaneStack NodePool. Use `spec.scheduling` only for
custom node selector or toleration overrides.

### Stage 3: Enterprise Scale

Pin provider package versions with `spec.package.version` and keep provider
credentials in a stack-owned Secret or ExternalSecret.

### Stage 4: Import Existing

Existing target-cluster ProviderConfigs can be left in place by omitting
`spec.secretName` where ProviderConfig rendering is optional.

AWS provider families are selected with `spec.providers`:

```yaml
spec:
  providers:
    iam: {}
    s3: {}
    ec2:
      runtimeConfig:
        requestsCpu: 500m
```

## Status

- `status.provider.ready`: readiness of the composed provider package Object.

## Composed Resources

- `Object` wrapping `DeploymentRuntimeConfig` for provider runtime settings.
- `Object` wrapping the target `Provider` package.

## Development

```sh
make render:all
make validate:all
make test
```

## License

Apache-2.0
