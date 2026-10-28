# Security policy

OrgExpenses is a development/demo project and is not currently suitable for
handling real financial or employee data without additional security work.
See the project status and limitations in [README.md](README.md).

## Reporting a vulnerability

Please do not publish exploit details in an issue. Report suspected
vulnerabilities through
[GitHub's private security advisory form](../../security/advisories/new).
Include the affected area, impact, and a concise reproduction description.
Do not include real user data or active credentials.

## Deployment notes

- Keep `.env`, API keys, mail credentials, and application keys out of Git.
- Use HTTPS and a production mail provider.
- Do not expose a debug-enabled application to the public internet.
- Review authorization, tenant isolation, OTP protections, and uploaded-file
  handling before deployment.
