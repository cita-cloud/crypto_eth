# Crypto Eth Agent

## 1. Project Basic Information
- **Project Name**: crypto_eth
- **Description**: A cryptographic service implementation for the CITA-Cloud platform, adopting Ethereum-compatible standards (secp256k1 for signatures and Keccak for hashing).
- **License**: Apache-2.0
- **Language**: Rust
- **Frameworks**: Tonic (gRPC), Tokio (Async Runtime), Cloud-Util (Common Utilities)

## 2. Core Capabilities

### 2.1 Cryptographic Operations
The service provides a gRPC interface (`CryptoService`) supporting the following operations:
- **Hashing**: Computes Keccak-256 hashes of data (`HashData`).
- **Signature Verification**: Verifies secp256k1 signatures (`VerifyDataHash` - *Note: function name suggests hash verification, but context implies signature/data integrity checks*).
- **Signing**: Signs messages using a local private key (`SignMessage`).
- **Recovery**: Recovers the public key/address from a signature (`RecoverSignature`).
- **Batch Verification**: Optimistically verifies a batch of transactions (`CheckTransactions`).

### 2.2 Observability & Maintenance
- **Health Check**: Implements the standard gRPC Health Check protocol.
- **Metrics**: Exposes Prometheus-compatible metrics via an HTTP server (default port 60005).
- **Logging**: Configurable logging via `log_config`.

## 3. Runtime Dependencies

### 3.1 Configuration
The service requires a TOML configuration file (default `config.toml`).

**Key Configuration Options:**
- `crypto_port` (Default: 50005): Port for the gRPC crypto service.
- `enable_metrics` (Default: true): Enable/disable metrics exporter.
- `metrics_port` (Default: 60005): Port for the metrics HTTP server.
- `domain`: Service domain name.
- `log_config`: Logging settings (level, rotation, etc.).

### 3.2 External Resources
- **Private Key File**: A file containing the private key for signing operations (path specified via CLI).

## 4. Usage Examples

### 4.1 Build
```bash
cargo build --release
```

### 4.2 Run
```bash
# Run with default configuration
./target/release/crypto run

# Run with custom config and private key
./target/release/crypto run -c config.toml -p private_key
```

### 4.3 Configuration Example (config.toml)
```toml
crypto_port = 50005
enable_metrics = true
metrics_port = 60005
domain = "crypto-eth"

[log_config]
log_level = "info"
```

## 5. API Reference (gRPC)
The service implements the `CryptoService` defined in `cita_cloud_proto`:

| Method | Description |
|--------|-------------|
| `GetCryptoInfo` | Returns metadata about the crypto module (name: "crypto_eth"). |
| `HashData` | Computes the hash of input data. |
| `VerifyDataHash` | Verifies if a hash matches the provided data. |
| `SignMessage` | Signs a message hash with the configured private key. |
| `RecoverSignature` | Recovers the address from a signature and message hash. |
| `CheckTransactions` | Verifies a stream/batch of raw transactions. |
