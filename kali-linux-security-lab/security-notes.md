# Security Notes

## Defensive Baseline

### Accounts

- Use a unique non-default account.
- Use strong, unique passwords.
- Prefer SSH keys for remote administration.
- Disable remote root login.
- Review privileged group membership.
- Remove unused accounts.

Checks:

```bash
id
getent group sudo
sudo lastlog
```

### Updates

```bash
sudo apt update
apt list --upgradable
```

Record update date and unresolved package issues.

### Services

```bash
systemctl --failed
sudo ss -lntup
```

Every listening service should have a documented purpose.

### Firewall

```bash
sudo ufw status verbose 2>/dev/null
sudo nft list ruleset 2>/dev/null
```

Apply least-access rules suited to the isolated lab.

### SSH

Review:

- Root login.
- Password authentication.
- Authorized keys.
- Source network restrictions.
- Log entries.
- Recovery access.

### Storage and Evidence

- Encrypt sensitive working data.
- Use synthetic credentials and files.
- Do not store real client or household traffic.
- Exclude captures and secrets with `.gitignore` if later added.
- Securely remove temporary artifacts when no longer required.

## Security Observation Template

```text
Observation ID:
Date:
Asset:
Description:
Evidence:
Risk level:
Recommended action:
Owner:
Target date:
Validation:
Status:
```

## Example Defensive Finding

```text
Observation ID: LAB-001
Asset: Ubuntu test server
Description: SSH password authentication remained enabled after key-based access was configured.
Evidence: Authorized configuration review.
Risk level: Medium in this lab context.
Recommended action: Verify key login, disable password authentication, validate syntax, reload SSH, and retest from a second terminal.
Validation: Successful key login; password authentication rejected.
Status: Closed.
```

## Lessons-Learned Prompts

- Was the scope specific enough?
- Did the lab generate any unintended traffic?
- Which logs were most useful?
- Which finding had the highest defensive value?
- Was remediation practical?
- Did the retest prove improvement?
- What evidence was excluded for privacy?
- What should be automated next?

## Ethics Statement

Security tools do not create authorization. The owner of a system must explicitly permit testing. Technical ability should be paired with restraint, documentation, privacy, and responsibility.
