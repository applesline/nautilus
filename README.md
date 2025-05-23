# Nautilus: Verifiable offchain computation on Sui

Nautilus is a framework for **secure and verifiable off-chain computation on Sui**. It allows developers to delegate sensitive or resource-intensive tasks to a self-managed [Trusted Execution Environment (TEE)](https://en.wikipedia.org/wiki/Trusted_execution_environment) while preserving trust on-chain through smart contract-based verification.

Nautilus is designed for hybrid decentralized applications (Dapps) that require private data handling, complex computation, or integration with external (Web2) systems. It ensures that off-chain computations are tamper-resistant, isolated, and cryptographically verifiable. 

The initial release supports **self-managed** [AWS Nitro Enclave TEEs](https://aws.amazon.com/ec2/nitro/nitro-enclaves/). Developers can verify AWS-signed enclave attestations on-chain using Sui smart contracts written in Move.

> [!IMPORTANT]
> Nautilus is available in beta with Sui Testnet. The Mainnet release is yet to be scheduled.

## Features

A Nautilus application consists of two components:

- Offchain server: Runs inside a TEE, like AWS Nitro Enclaves, and performs the actual computation, such as processing user input or executing a scheduled task.
- Onchain smart contract: Written in Move, this contract receives the output and verifies the TEE's attestation before trusting or acting on the data.

> Note: We chose to initially support AWS Nitro Enclaves because of its maturity and reproducibility. We will consider adding more TEE providers in the future.

**How it works**

- Deploy the offchain server to a self-managed TEE, such as AWS Nitro Enclaves. You may or may not use the reproducible build template available in this repo.
- The TEE generates a cryptographic attestation that proves the integrity of the execution environment.
- Sui smart contracts verify the attestation onchain before accepting the TEE output.
- The integrity of the TEE is auditable and anchored by the provider’s root of trust.

> [!IMPORTANT]
> The provided reproducible build template is intended as a starting point for building your own enclave. It is not feature-complete, has not undergone a security audit, and is offered as a modification-friendly reference licensed under the Apache 2.0 license. THE TEMPLATE AND ITS RELATED DOCUMENTATION ARE PROVIDED `AS IS` WITHOUT WARRANTY OF ANY KIND FOR EVALUATION PURPOSES ONLY.
> We encourage you to adapt and extend it to fit your specific use case.

## Use cases

Several Web3 use cases can use Nautilus for trustworthy and verifiable offchain computation. Examples include:

- **Trusted Oracles**: Nautilus could ensure that oracles fetch and process offchain data in a tamper-resistant manner before providing results to a smart contract. The source of external data could be a Web2 service (like weather, sports, betting, asset prices, etc.) or a decentralized storage platform like [Walrus](https://walrus.xyz).
- **AI agents**: Nautilus would be ideal to securely run AI models for inference or execute agentic workflows to produce actionable outcomes, while providing data & model provenance onchain.
- **DePIN solutions**: DePIN (Decentralized Physical Infrastructure) could leverage Nautilus for private data computation in IoT and supply chain networks.
- **Fraud prevention in multi-party systems**: Decentralized exchanges (DEXs) could use Nautilus for order matching and settlement, or layer-2 solutions could prevent collision & fraud by securely running computations between untrusted parties.
- **Identity management**: Solutions in the identity management space that require onchain verifiability for decentralized governance and proof of tamper-resistance, could utilize Nautilus.
- and more…

When used together, Nautilus and [Seal](https://github.com/MystenLabs/seal) enable powerful privacy-preserving use cases by combining secure & verifiable computation with secure key access. A common challenge with TEEs is persisting secret keys across restarts and different machines. Seal can address this by securely storing long-term keys and granting access only to properly attested TEEs. In this model, Nautilus handles computation over the encrypted data, while Seal controls key access. Applications that require a shared encrypted state can use both tools to privately process user requests and update encrypted data on public networks.

## Future plans and non-goals

We plan to expand Nautilus support to additional TEE providers in the future, such as [Intel TDX](https://www.intel.com/content/www/us/en/developer/tools/trust-domain-extensions/overview.html) and [AMD SEV](https://www.amd.com/en/developer/sev.html). We welcome feedback from the builder community on which platforms to prioritize, or suggestions for others to consider.

Currently, Nautilus does not aim to provide a readily usable TEE network. Developers are encouraged to deploy and manage their own TEEs for running off-chain Nautilus servers.

## Contact Us
For questions about Nautilus, use case discussions, or integration support, contact the Nautilus team on [Sui Discord](https://discord.com/channels/916379725201563759/1361500579603546223).

## More information 
- [Nautilus Design](Design.md)
- [Using Nautilus](UsingNautilus.md)
- [LICENSE](LICENSE)
