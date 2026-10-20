# Terraform LSP Plugin for Claude Code

A [Claude Code](https://claude.com/claude-code) plugin that integrates [terraform-ls](https://github.com/hashicorp/terraform-ls) (HashiCorp's official Terraform language server) for code intelligence, diagnostics, and completions in Terraform files.

## How This Differs from the Terraform MCP Plugin

The official [Terraform MCP plugin](https://github.com/anthropics/claude-plugins-public/tree/main/external_plugins/terraform) by HashiCorp provides automation capabilities — running plans, managing state, and interacting with the Terraform ecosystem.

This plugin is complementary. It provides **language intelligence** while you edit — diagnostics, completions, hover documentation, and code navigation for `.tf` and `.tfvars` files. Both plugins can be installed together.

| | Terraform MCP Plugin | Terraform LSP Plugin (this) |
| --- | --- | --- |
| **Type** | MCP server | LSP integration |
| **Provides** | Plan/apply automation, state management | Diagnostics, completions, hover docs |
| **When it helps** | Running and managing infrastructure | Writing and editing Terraform code |

## Supported File Types

| Extension | Language ID |
| --------- | ----------- |
| `.tf` | `terraform` |
| `.tfvars` | `terraform-vars` |

## Prerequisites

> **Important:** `terraform-ls` must be installed and on your PATH **before** using this plugin. Without it, the language server will fail to start and you will not receive diagnostics or completions for Terraform files. Claude Code does not install language servers automatically.

Install `terraform-ls` on your system.

### macOS (Homebrew)

```sh
brew install hashicorp/tap/terraform-ls
```

### Linux

Download the latest release from [HashiCorp Releases](https://releases.hashicorp.com/terraform-ls/) and place the binary on your PATH.

### Windows

Download from [HashiCorp Releases](https://releases.hashicorp.com/terraform-ls/) and add to your PATH.

### Verify Installation

```sh
terraform-ls -v
```

## Install Plugin

Once the plugin is listed in a marketplace, install with:

```sh
claude plugin install terraform-lsp
```

For local development, use the `--plugin-dir` flag:

```sh
claude --plugin-dir /path/to/claude-terraform-lsp-plugin
```

## Usage

Once installed, Claude Code starts the `terraform-ls` language server when it encounters `.tf` or `.tfvars` files. The language server provides:

- **Diagnostics** — syntax errors, missing references, validation
- **Completions** — resource types, attribute names, built-in functions
- **Hover** — documentation for resources and attributes

## Examples

### Catching errors immediately after edits

Ask Claude to add an `aws_security_group` resource to a `.tf` file. After writing the file, terraform-ls publishes diagnostics — for instance, flagging a reference to a VPC that doesn't exist in the configuration, or a syntax error in an HCL expression. Claude sees the diagnostic immediately and corrects the issue without needing to run `terraform validate` or `terraform plan`.

### Writing accurate Terraform with provider-aware completions and hover

Ask Claude to create an RDS database instance. With the plugin enabled, terraform-ls provides completions for valid `aws_db_instance` attributes and hover documentation for each one. Claude uses this to write correct HCL with the right attribute names and types, rather than relying solely on training data that may be outdated for newer provider versions.

### Navigating a large Terraform codebase with go-to-definition

In a multi-file Terraform project, ask Claude to refactor a module or update a variable used across several files. The plugin provides go-to-definition and find-references, so Claude can trace where `var.environment` is declared, where it's used, and what resources depend on it — making the refactor accurate across the entire codebase.

## Troubleshooting

### Language server not starting

Verify `terraform-ls` is installed and on your PATH:

```sh
which terraform-ls && terraform-ls -v
```

If the command is not found, follow the [Prerequisites](#prerequisites) section above.

### Limited completions or missing provider attributes

- Run `terraform init` in your workspace — `terraform-ls` uses the provider schemas from `.terraform/` to power completions and attribute validation. Without it, you'll get basic syntax support but not full code intelligence.

## License

Apache 2.0 — see [LICENSE](LICENSE).
