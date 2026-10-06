# Cloud Application Security PoC

A small proof of concept demonstrating how application and container
security controls can be integrated directly into a CI/CD pipeline.

## Architecture

Developer

GitHub

GitHub Actions

Docker Build

Trivy Vulnerability Scan (SCA)

Security Gate

Pass / Fail

## Security Controls

- Container vulnerability scanning with Trivy
- Vulnerability policy enforcement
- Automated security scanning on every push
- Docker containerisation
- Dependency remediation
- Targeted handling of scanner false positives

## Vulnerability Detection

The initial container security scan identified actionable high severity
vulnerabilities.

The CI/CD pipeline was configured to return a non-zero exit code when
high or crits vulnerabilities with available fixes were detected,
preventing the insecure build from progressing.

### Before remediation

4 critical vulns found

## Remediation

The vulnerable dependencies were upgraded:

- `msgpack` 1.1.2 → 1.2.3
- `setuptools` 70.3.0 → 84.0.0
- `urllib3` 2.7.0 → 2.8.0

The rebuilt container was then rescanned.

Residual duplicate findings originating from pip's embedded SBOM were
investigated and handled using a targeted scanner exclusion rather than
broadly suppressing vulnerabilities.

### After remediation

All Vulns idenfited and remediated

## Result

The resulting security pipeline now automatically:

1. Checks out the application source.
2. Builds the Docker container.
3. Scans the image for HIGH and CRITICAL vulnerabilities.
4. Blocks builds containing actionable security findings.
5. Allows remediated builds to progress.

## Enterprise / Wiz Mapping

This PoC uses open-source tooling because I do not have access to a peronsal Wiz
tenant.

The same security engineering pattern can be implemented with Wiz Code
to integrate security controls into the development lifecycle and provide
additional code to cloud context and prioritisation.