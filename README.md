# awsx

Tiny shared plumbing for the [AWS SDK for Go v2](https://github.com/aws/aws-sdk-go-v2): one place to
resolve credentials and build the per-service clients an app actually uses, so callers don't each
reinvent it.

- **Explicit or ambient credentials.** Pass a static access/secret key (a scoped, non-interactive
  identity injected as config), or supply nothing and fall back to the AWS default chain (SSO in
  dev, instance role in production). An unset pair is a no-op.
- **Service client builders** with sensible defaults — e.g. the Price List client pins the global
  `us-east-1` endpoint and uses adaptive retry (that API throttles a broad sweep).

## Install

```
go get github.com/getotium/awsx
```

## Usage

```go
// Resolve a config (static creds if given, else the default chain).
cfg, err := awsx.LoadConfig(ctx, "us-east-1", awsx.WithStaticCredentials(accessKey, secretKey))

ec2Client     := awsx.EC2(cfg, "eu-west-1") // per-region EC2 client
pricingClient := awsx.Pricing(cfg)          // Price List client (global, adaptive retry)
```

## Provenance

Extracted from [Otium](https://getotium.com), where it backs the AWS pricing and capacity sources.
Deliberately minimal — credential resolution + client construction, nothing more.

## License

[Apache-2.0](LICENSE).
