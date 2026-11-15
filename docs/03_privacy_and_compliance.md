# 3. Privacy and compliance

## 3.1 Data protection

- **Minimisation before the AI:** contact details, the candidate's name and protected attributes are removed by
  the guard. The AI sees what the assessment needs.
- **Isolation:** each client runs in its own backend project. A security check fails the build if the app's public
  key can read any table or the CV storage, or if one client's user can reach another client's project. It runs
  automatically before a client build is packaged, so a build cannot ship while data is open.
- **Access:** sign-in with a second factor in client builds; database rules serve data only to sessions that passed
  it. Local and demo modes never touch the client backend.
- **Secrets:** the AI key lives in the operating system's keychain locally, or as a secret of the client's backend
  project. It is never in the app, the repository or an environment file shipped with the app.
- **Retention:** candidates are deleted automatically when their retention period ends. Extension is per candidate
  and logged.
- **Records:** a data-processing information pack, a security-hardening record, an incident log and a verification
  checklist are maintained.

## 3.2 EU AI Act

AI used to recruit or select people, in particular to filter applications and evaluate candidates, is high-risk
under Annex III. The obligations apply from 2 December 2027. The position below is the author's research, not
legal advice, and a lawyer should confirm it.

| Duty | Status |
|---|---|
| Risk management | Risk register in place; reviewed per release |
| Data governance and bias examination | Bias lab with synthetic data only (see [02](02_fairness_testing.md)) |
| Technical documentation | Outline |
| Record-keeping | Done: per assessment and per decision, append-only audit log |
| Instructions for use | Template filled in |
| Human oversight | Done: own view before the AI, single queue, own words, re-review, oversight metrics |
| Accuracy, robustness, cybersecurity | Guard, injection outcome test, security hardening. **Accuracy metrics still to define** |
| Quality management | Lightweight process |
| Conformity assessment, declaration, registration | **To do** before December 2027 |
| Post-market monitoring | Incident log exists; monitoring plan in the technical documentation |

Bias testing uses synthetic CVs only, and no real demographic data is sent to the AI provider.
