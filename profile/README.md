<p align="center">
  <img src="https://raw.githubusercontent.com/ingo-os/.github/main/profile/assets/logo.png" width="200" alt="Ingo OS Logo">
</p>

<h1 align="center">Ingo OS</h1>

<p align="center">
  <strong>A Cognitive Home Operating Environment</strong>
</p>

<p align="center">
  <a href="https://opensource.org/licenses/MIT">
    <img src="https://img.shields.io/badge/License-MIT-blue.svg" alt="License: MIT">
  </a>
  <a href="https://www.rust-lang.org">
    <img src="https://img.shields.io/badge/Rust-1.75%2B-orange.svg" alt="Rust">
  </a>
  <a href="https://flutter.dev">
    <img src="https://img.shields.io/badge/Flutter-3.16%2B-cyan.svg" alt="Flutter">
  </a>
</p>

<p align="center"><i>"The home should anticipate your needs, not just respond to your commands."</i></p>

## What is Ingo OS?

Ingo OS is a **distributed, AI-native home automation platform** that transforms your house into a cognitive environment. Unlike traditional systems that simply connect devices, Ingo OS:

- **Understands natural language** - Talk to your home like a person
- **Learns your preferences** - Adapts automatically over time
- **Negotiates between goals** - Balances comfort, energy, and security
- **Runs entirely local** - Your data never leaves your home
- **Thinks ahead** - Proactive, not just reactive

## Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                        INGO OS                                  │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   ┌─────────────┐   ┌─────────────┐   ┌─────────────┐          │
│   │   GUI       │   │   Voice     │   │   Mobile    │          │
│   │  (Flutter)  │   │  (Whisper)  │   │   (App)     │          │
│   └──────┬──────┘   └──────┬──────┘   └──────┬──────┘          │
│          └─────────────────┴─────────────────┘                  │
│                           │                                     │
│   ┌───────────────────────┴───────────────────────┐            │
│   │              ingo-api (gRPC/WS)               │            │
│   └───────────────────────┬───────────────────────┘            │
│                           │                                     │
│   ┌───────────────────────┴───────────────────────┐            │
│   │            ingo-daemon (Orchestrator)         │            │
│   └──────┬────────┬────────┬────────┬─────────────┘            │
│          │        │        │        │                          │
│   ┌──────┴──┐ ┌───┴───┐ ┌──┴───┐ ┌─┴─────┐  ┌─────────┐      │
│   │  Agent  │ │ Logic │ │ Core │ │  HAL  │  │   Bus   │      │
│   │ (AI)    │ │(Rules)│ │(State│ │(Zigbee│  │ (NATS)  │      │
│   │         │ │       │ │      │ │Z-Wave │  │         │      │
│   └─────────┘ └───────┘ └──────┘ └───────┘  └─────────┘      │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

## Key Features

### 🧠 Cognitive Layer
- **Local LLM inference** (Llama 3.1, Mistral) - no cloud required
- **Natural language understanding** - "I'm cold" → adjusts heating
- **Continual learning** - Adapts to your patterns via DPO/RLHF
- **Multi-agent negotiation** - Agents coordinate for optimal outcomes

### 🏠 Universal Device Support
- Zigbee 3.0, Z-Wave Plus, WiFi, Bluetooth LE
- Philips Hue, IKEA TRÅDFRI, Tasmota, ESPHome
- Automatic device discovery and capability detection

### 🔒 Privacy-First
- 100% on-device processing
- End-to-end encrypted communication
- No cloud dependencies
- Open source - audit the code

### ⚡ High Performance
- Rust-based core - memory safe, zero-cost abstractions
- CRDT-based state - conflict-free distributed updates
- Sub-10ms rule execution, sub-100ms AI inference

## Quick Start

### Installation

```bash
# Clone the repository
git clone https://github.com/ingo-os/ingo.git
cd ingo

# Build and install
cargo build --release
sudo cp target/release/ingo-daemon /usr/local/bin/

# Start NATS (message bus)
docker run -d --name nats -p 4222:4222 nats:latest

# Run the daemon
ingo-daemon --config ./config/example.toml
```

### First Command

```bash
# Using CLI
ingo device list
ingo agent "Turn on the living room lights"

# Using GUI
# Open http://localhost:8080 in your browser
```

## Repository Structure

| Directory | Description |
|-----------|-------------|
| `crates/ingo-core` | Core types, CRDTs, state registry |
| `crates/ingo-bus` | NATS message bus |
| `crates/ingo-hal` | Hardware abstraction (Zigbee, Z-Wave, etc.) |
| `crates/ingo-logic` | Rule engine for deterministic behavior |
| `crates/ingo-agent` | AI agents with local LLM inference |
| `crates/ingo-api` | gRPC and WebSocket gateway |
| `crates/ingo-daemon` | Main system orchestrator |
| `crates/ingo-cli` | Command-line interface |
| `gui/` | Flutter-based GUI |
| `proto/` | Protocol buffer definitions |

## Hardware Requirements

| Component | Minimum | Recommended |
|-----------|---------|-------------|
| CPU | 4 cores | 8+ cores |
| RAM | 4 GB | 16 GB |
| GPU | - | NVIDIA 8GB+ or Apple Silicon |
| Storage | 10 GB | 100 GB SSD |
| Network | WiFi/Ethernet | Ethernet preferred |

## Supported Protocols

- ✅ Zigbee 3.0 (via zigbee2mqtt)
- ✅ Z-Wave Plus (via zwave-js)
- ✅ WiFi/MQTT (Tasmota, ESPHome)
- ✅ Bluetooth LE
- 🔄 Thread/Matter (in development)

## Documentation

- [Architecture Overview](docs/architecture.md)
- [API Reference](docs/api.md)
- [Device Integration](docs/devices.md)
- [Contributing](CONTRIBUTING.md)

## Community

- 💬 [Discussions](https://github.com/ingo-os/ingo/discussions)
- 🐛 [Issue Tracker](https://github.com/ingo-os/ingo/issues)
- 📖 [Wiki](https://github.com/ingo-os/ingo/wiki)

## Roadmap

### Phase 1: Foundation ✅
- [x] Core state management
- [x] Hardware abstraction
- [x] Rule engine
- [x] Basic GUI

### Phase 2: Intelligence 🔄
- [x] Local LLM integration
- [x] Multi-agent system
- [x] Learning pipeline
- [ ] Voice assistant

### Phase 3: Scale 📋
- [ ] Multi-home federation
- [ ] Advanced analytics
- [ ] Mobile apps
- [ ] Plugin ecosystem

## Contributing

We welcome contributions! See [CONTRIBUTING.md](../CONTRIBUTING.md) for guidelines.

### Development Setup

```bash
# Install Rust
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

# Install dependencies
cargo install cargo-watch cargo-tarpaulin

# Run tests
cargo test --workspace

# Run with hot reload
cargo watch -x 'run --bin ingo-daemon'
```

## License

MIT License - see [LICENSE](../LICENSE)

---

<p align="center">
  <strong>Built with ❤️ by the Ingo OS community</strong>
</p>
