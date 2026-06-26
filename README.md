# MetaEngine Protobuf C# HttpClient

[![NuGet version](https://img.shields.io/nuget/v/MetaEngine.CSharp.Protobuf.HttpClient.Tool.svg)](https://www.nuget.org/packages/MetaEngine.CSharp.Protobuf.HttpClient.Tool/)
[![NuGet downloads](https://img.shields.io/nuget/dt/MetaEngine.CSharp.Protobuf.HttpClient.Tool.svg)](https://www.nuget.org/packages/MetaEngine.CSharp.Protobuf.HttpClient.Tool/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**Generate idiomatic C# clients and models from Protobuf (`.proto`) definitions.**

`HttpClient`-based gRPC-Connect services with `System.Text.Json` models — or binary gRPC clients via `Grpc.Net.Client` + protobuf-net — typed, with optional XML docs, DataAnnotations validation from protovalidate, retries, auth, and DI registration.

> Distributed as a **.NET tool** — `dotnet tool install` and run it from the command line. No project reference, no MSBuild wiring; it uses the .NET SDK you already have.

---

## Quick Links

- **NuGet Package**: [MetaEngine.CSharp.Protobuf.HttpClient.Tool](https://www.nuget.org/packages/MetaEngine.CSharp.Protobuf.HttpClient.Tool)
- **Website**: [metaengine.eu](https://www.metaengine.eu)

---

## Features

- ✅ **Native dotnet tool** - `dotnet tool install` and run; no project reference, no build-time wiring
- ✅ **gRPC-Connect services** - One typed `HttpClient` service per gRPC service (Connect-RPC over JSON)
- ✅ **Binary gRPC option** - `--grpc-binary` switches to `Grpc.Net.Client` + protobuf-net
- ✅ **`System.Text.Json` models** - Records/classes with typed members and standalone enums
- ✅ **XML docs** - Optional `/// <summary>` doc comments from proto comments
- ✅ **DataAnnotations** - Optional `[Required]`, `[StringLength]`, `[Range]`, ... from protovalidate (`buf.validate`) constraints
- ✅ **DI registration** - Optional `IServiceCollection` extension methods for typed `HttpClient`s
- ✅ **Type mapping** - Override protobuf well-known type mappings to .NET types
- ✅ **Auth & resilience** - Bearer/basic auth, custom headers, retries with exponential backoff
- ✅ **Type Filtering** - Generate only the messages/enums you need

---

## Installation

```bash
# Global (available everywhere)
dotnet tool install -g MetaEngine.CSharp.Protobuf.HttpClient.Tool

# Or pin it to a repo (local tool manifest)
dotnet new tool-manifest        # once per repo
dotnet tool install MetaEngine.CSharp.Protobuf.HttpClient.Tool
```

---

## Requirements

- **.NET SDK 8.0** or later — the same toolchain you use to build C#. The tool runs on any platform .NET supports (Linux, macOS, Windows).

---

## Quick Start

```bash
metaengine-protobuf-csharp-httpclient <input> <output> <namespace> [options]
```

### Recommended Setup

```bash
metaengine-protobuf-csharp-httpclient ./service.proto ./Generated Shop.Client \
  --documentation \
  --dependency-injection
```

### More Examples

```bash
# Binary gRPC client (Grpc.Net.Client + protobuf-net) with retries
metaengine-protobuf-csharp-httpclient service.proto ./Generated Shop.Client \
  --grpc-binary --retries 3

# Override a well-known type mapping + auth
metaengine-protobuf-csharp-httpclient service.proto ./Generated Shop.Client \
  --type-mapping google.protobuf.Timestamp=System.DateTime --bearer-auth API_TOKEN
```

---

## CLI Options

| Argument / Option | Description | Default |
|---|---|---|
| `input` *(required)* | Protobuf (`.proto`) file path or inline proto content | - |
| `output` *(required)* | Output directory for generated C# files | - |
| `namespace` *(required)* | Root namespace for the generated client and models | - |
| `--documentation` | Generate XML doc comments from proto comments | `false` |
| `--middleware` | Generate middleware infrastructure (`ApiDelegatingHandler` + `ServiceCollectionExtensions`) | `false` |
| `--validation-annotations` | Emit `System.ComponentModel.DataAnnotations` attributes from protovalidate (`buf.validate`) constraints | `false` |
| `--bearer-auth [env-var]` | Bearer token from an env var (default `API_TOKEN`); adds an `Authorization` header | - |
| `--basic-auth` | Basic auth from env vars (`API_USERNAME` / `API_PASSWORD`) | `false` |
| `--retries [max-attempts]` | Enable retries with exponential backoff; optional value sets max attempts | - |
| `--custom-header <name=env-var>` | Static header from an env var. Repeatable | - |
| `--grpc-binary` | Generate binary gRPC clients (`Grpc.Net.Client` + protobuf-net) instead of Connect-RPC JSON | `false` |
| `--dependency-injection` | Generate `IServiceCollection` extension methods for DI registration of typed `HttpClient`s | `false` |
| `--type-mapping <protobuf.Type=DotNetType>` | Override a protobuf well-known type mapping to a fully-qualified .NET type (e.g. `google.protobuf.Timestamp=System.DateTime`). Repeatable | - |
| `--include-types <types>` | Only generate these proto types (comma-separated message/enum names) | - |
| `--clean` | Clean the destination directory before generating | `false` |
| `--verbose` | Enable verbose logging | `false` |

> **Note** — gRPC-Connect surfaces errors as gRPC status codes parsed into a typed `GrpcException`, so there is no HTTP-status `--error-handling` flag: error handling is unconditional engine behavior.

---

## Generated Code Structure

```
output/
  ├── Models/                    # One file per proto message/enum (System.Text.Json)
  │   ├── Order.cs               # public class Order { ... }
  │   ├── OrderStatus.cs         # standalone enum type
  │   └── ...
  ├── Client/                    # One service per gRPC service
  │   ├── OrderServiceClient.cs  # all RPCs on OrderService
  │   └── ...
  └── BaseHttpClient.cs          # shared gRPC-Connect / gRPC plumbing
```

---

## Support

- **Issues**: [GitHub Issues](https://github.com/meta-engine/protobuf-csharp-httpclient/issues)
- **Email**: info@metaengine.eu
- **Website**: [metaengine.eu](https://www.metaengine.eu)

---

## License

MIT License - see [LICENSE](./LICENSE) file for details.

---

## About This Repository

This is the **documentation and issue tracking repository** for MetaEngine Protobuf C# HttpClient. The package is published to [NuGet.org](https://www.nuget.org/packages/MetaEngine.CSharp.Protobuf.HttpClient.Tool).

Source code is proprietary, but the package is free to use under the MIT license.
