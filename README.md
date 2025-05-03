# setup-nix

[![GitHub: Release on Push](../../actions/workflows/release-on-push.yml/badge.svg)](../../actions/workflows/release-on-push.yml)

Set up a GitHub Actions Workflow with a Specific Version of Nix.

## 🔣 Inputs

The GitHub Action Workflow `sentryeagle/setup-nix` accepts the following inputs:

<table>
    <thead>
        <th>Name</th>
        <th>Description</th>
        <th>Required/Optional</th>
        <th>Default</th>
        <th>Example</th>
    </thead>
    <tbody>
        <tr>
            <td>
                <code>install_url</code>
            </td>
            <td>Set URL pointing to a Nix installer script. Helpful for testing or pinning Nix to a specific version.</td>
            <td>Optional</td>
            <td align="center">-</td>
            <td align="center">
                <code>https://releases.nixos.org/nix/nix-2.28.3/install</code>
            </td>
        </tr>
        <tr>
            <td>
                <code>install_options</code>
            </td>
            <td>Set Nix installation options. These are passed to the Nix installer script.</td>
            <td>Optional</td>
            <td align="center">-</td>
            <td align="center">
                <code>--no-daemon</code>
            </td>
        </tr>
        <tr>
            <td>
                <code>nix_path</code>
            </td>
            <td>Set <code>NIX_PATH</code> environment variable.</td>
            <td>Optional</td>
            <td align="center">-</td>
            <td align="center">
                <code>nixpkgs=channel:nixos-unstable</code>
            </td>
        </tr>
        <tr>
            <td>
                <code>extra_nix_config</code>
            </td>
            <td>Set extra Nix configuration to append to <code>/etc/nix/nix.conf</code>.</td>
            <td>Optional</td>
            <td align="center">-</td>
            <td>
                <pre>
extra_nix_config: |
    ...
                </pre>
            </td>
        </tr>
        <tr>
            <td>
                <code>enable_kvm</code>
            </td>
            <td>If available, enable KVM for hardware-accelerated virtualization on Linux. </td>
            <td>Optional</td>
            <td align="center">
                <code>true</code>
            </td>
            <td align="center">
                <code>false</code>
            </td>
        </tr>
        <tr>
            <td>
                <code>set_current_user_as_trusted_user</code>
            </td>
            <td>Set current user as <code>trusted-users</code>.</td>
            <td>Optional</td>
            <td align="center">
                <code>true</code>
            </td>
            <td align="center">
                <code>false</code>
            </td>
        </tr>
        <tr>
            <td>
                <code>github_access_token</code>
            </td>
            <td>Set the GitHub access token to configure Nix to pull from GitHub. Helpful to work around rate limiting issues. </td>
            <td>Optional</td>
            <td align="center">-</td>
            <td align="center">
                <code>${{ secrets.GITHUB_TOKEN }}</code>
            </td>
        </tr>
    </tbody>
</table>

## ⚙️ Usage

Create a `.github/workflows/setup-nix.yaml` in your GitHub Repository with the following contents:

```yaml
name: "Setup Nix"

on: [push]

permissions:
    contents: "read"

jobs:
    example:
        runs-on: "ubuntu-latest"

        steps:
            - name: "GitHub: Checkout Repository"
              uses: "actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683" #v4.2.2

            - name: "GitHub: Setup Nix"
              # IMPORTANT: 
              # Consider GitHub Actions Pinning for additional security.
              # See: https://stackoverflow.com/a/78905195
              uses: "sentryeagle/setup-nix@latest"
              with:
                # NOTE:
                # Set 'nix_path' by:
                #  - Picking a channel → https://status.nixos.org/
                #  - Pin 'nixpkgs' → https://nix.dev/reference/pinning-nixpkgs
                nix_path: nixpkgs=channel:nixos-unstable

            - name: "Nix: Build"
              run: |
                nix-build
```

## 🐛 Found a Bug?

Thank you for your message! Please fill out a [bug report](../../issues/new?assignees=&labels=&template=bug_report.md&title=).

## 📖 License

This project is licensed under the [European Union Public License 1.2](https://interoperable-europe.ec.europa.eu/sites/default/files/custom-page/attachment/2020-03/EUPL-1.2%20EN.txt).