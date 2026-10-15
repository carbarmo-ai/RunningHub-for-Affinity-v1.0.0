# Security and Privacy

- Keep your RunningHub API Key private. The Windows helper stores it using Windows DPAPI for the current user.
- **Never publish generated Affinity `.js` scripts**: they contain a local authentication token and may include user-specific paths.
- Do not commit `credencial.dpapi`, `bridge_token.txt`, `config.json`, `workflow_profiles.json` containing private data, logs, images, or Exchange job folders.
- The local connector listens on `127.0.0.1:8765`; do not expose it to external networks.
- Submitted images and workflow inputs are sent to RunningHub. Review RunningHub's terms and privacy practices.
- Download the Windows installer only from the project's official GitHub Releases.

## Reporting vulnerabilities

Please do not post credentials or exploit details in public issues. Contact the maintainer privately through the security contact listed in the repository once configured.

**Security contact:** [ADD CONTACT BEFORE PUBLICATION]
