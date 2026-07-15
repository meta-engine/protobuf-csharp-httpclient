# Changelog

See the [NuGet versions page](https://www.nuget.org/packages/MetaEngine.CSharp.Protobuf.HttpClient.Tool#versions-body-tab) for the full version history.

## 1.0.1

### Bug Fixes

- **`--documentation` comments bind to their member.** A property carrying both a doc comment and an attribute (e.g. with `--validation-annotations`) emitted the `/// <summary>` between the attribute and the property, leaving it orphaned. The doc comment now renders above the attributes.
- **Consistent indentation in generated client methods.** Some service method bodies were dedented; all generated client code is now uniformly indented.

## 1.0.0

- Initial release — C# code generation from Protobuf (`.proto`) definitions, distributed as a dotnet tool
- **gRPC-Connect services** — one typed `HttpClient` service per gRPC service (Connect-RPC over JSON)
- **`System.Text.Json` models** — typed members, standalone enum types
- `--grpc-binary` — binary gRPC clients via `Grpc.Net.Client` + protobuf-net
- `--documentation` — XML doc comments from proto comments
- `--validation-annotations` — `System.ComponentModel.DataAnnotations` from protovalidate (`buf.validate`) constraints
- `--dependency-injection` — `IServiceCollection` extension methods for DI
- `--type-mapping` — override protobuf well-known type mappings to .NET types
- `--middleware` — `ApiDelegatingHandler` + `ServiceCollectionExtensions`
- `--bearer-auth`, `--basic-auth`, `--custom-header`, `--retries`
- `--include-types`, `--clean`, `--verbose`
