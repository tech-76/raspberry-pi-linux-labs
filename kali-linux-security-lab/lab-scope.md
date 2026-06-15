# Defensive Security Lab Scope

## Rules of Engagement

This document must be completed before any lab activity.

## 1. Authorization

```text
Lab owner:
Authorized tester:
Authorization date:
Purpose:
Approval reference:
```

For a personal home lab, state that all listed systems are owned and controlled by you.

## 2. In-Scope Assets

| Asset | Address | OS | Purpose | Owner |
|---|---|---|---|---|
| Kali Pi | `192.168.56.10` | Kali Linux | Analysis workstation | Lab owner |
| Ubuntu target | `192.168.56.20` | Ubuntu | Test server | Lab owner |
| Windows target | `192.168.56.30` | Windows | Test client | Lab owner |

Use a dedicated private test subnet.

## 3. Out-of-Scope Assets

Explicitly exclude:

- Public internet hosts.
- ISP equipment not owned by the tester.
- Neighbouring networks.
- Employer or client systems.
- Personal devices not included in the test.
- Cloud services without written authorization.
- Production accounts and data.

## 4. Permitted Activities

- Host discovery within the listed lab subnet.
- Service identification on listed targets.
- Packet capture of tester-generated traffic.
- Log review.
- Configuration review.
- Patch and hardening validation.
- File-integrity demonstrations using non-sensitive files.
- Authentication review using accounts created for the lab.

## 5. Prohibited Activities

- Denial-of-service testing.
- Password attacks against real accounts.
- Phishing or social engineering.
- Persistence or stealth techniques.
- Malware deployment.
- Data destruction.
- Testing outside the scope.
- Capturing unrelated user traffic.
- Exploiting public or third-party systems.

## 6. Testing Window

```text
Start:
End:
Timezone:
Maximum duration:
Stop conditions:
```

Stop immediately if traffic reaches an unintended asset or the lab causes instability outside the test environment.

## 7. Data Handling

- Store evidence locally in an encrypted or access-controlled location.
- Minimize captured data.
- Delete packet captures after analysis when no longer needed.
- Do not commit packet captures, credentials, hashes, or secrets to Git.
- Sanitize screenshots.
- Use synthetic test data.

## 8. Success Criteria

- Scope followed with no unintended impact.
- In-scope hosts correctly identified.
- Services documented.
- At least one defensive improvement recommended.
- Improvement applied and retested.
- Evidence sanitized and summarized.

## 9. Incident and Stop Procedure

If an unintended system is contacted:

1. Stop the activity.
2. Disconnect the testing interface if necessary.
3. Record the time and command.
4. Preserve only minimal evidence.
5. Confirm no further traffic is being sent.
6. Update the scope or lab isolation before resuming.

## 10. Sign-Off

```text
I confirm that I own or have explicit permission to test every in-scope asset.

Name:
Date:
Signature or acknowledgment:
```
