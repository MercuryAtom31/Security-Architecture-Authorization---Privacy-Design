# Privacy and Internet Tracking

## Goal

Understand how internet tracking works, how identities are correlated, and how to design a privacy-friendly architecture for a global company.

## Tracking Methods Comparison

### Cookies

Cookies store identifiers in the browser for session continuity and personalization. They can be first-party or third-party.

How cookies contribute to identity correlation:
- Persistent identifiers can link repeated visits.
- Third-party cookies can connect behavior across multiple websites.
- Cookie IDs can be joined with account logins and purchase events.

### Browser Fingerprinting

Fingerprinting collects device and browser properties such as:
- Browser and version
- Operating system
- Screen resolution
- Language and timezone
- Installed fonts and rendering behavior

How fingerprinting contributes to identity correlation:
- It creates a probabilistic unique profile without storing client-side state.
- It can still re-identify users after cookies are cleared.
- It is difficult for users to detect and control.

### IP Tracking

IP tracking uses network source information to infer location and provider.

How IP tracking contributes to identity correlation:
- Repeated requests from similar IP ranges can link sessions.
- Geolocation and ASN metadata add context to other identifiers.
- Alone it is weaker, but with cookies/fingerprints it becomes strong.

## Identity Correlation Summary

Correlation becomes highly effective when trackers combine multiple signals:
- Cookie ID + fingerprint + recurring IP patterns
- Timestamp and behavior patterns
- Device characteristics and account actions

Even if one signal changes, remaining signals can reconnect user activity.

## Diagrams

- [Tracking comparison diagram](../diagrams/tracking-comparison.puml)
- [Identity correlation diagram](../diagrams/identity-correlation.puml)

## Privacy-Friendly Architecture

### 1) Anonymization Layer

- Replace direct identifiers with pseudonymous internal IDs.
- Mask or generalize sensitive fields in analytics outputs.
- Separate identity keys from behavior datasets.

### 2) Tokenization

- Tokenize high-risk fields (email, phone, payment identifiers).
- Store mapping only in a hardened token vault with strict access controls.
- Rotate and monitor token access.

### 3) Minimal Data Collection

- Collect only data required for defined business purposes.
- Avoid unnecessary retention of raw IP and high-entropy fingerprints.
- Enforce short retention windows and automatic deletion.

### Supporting Controls

- Consent and preference management by region.
- Purpose-based access controls for downstream teams.
- Immutable audit logs for accountability.

## Compliance Discussion

### GDPR Alignment

- Data minimization and purpose limitation are enforced at collection.
- Consent is explicit where required and can be withdrawn.
- Data subject rights (access, deletion, correction) are operationalized.
- Privacy by design is implemented through layered controls.

### CCPA Alignment

- Clear notice of collection categories and purposes.
- Opt-out mechanisms for sharing/sale contexts.
- User rights to know and delete are supported by data workflows.
- Access control and auditability support accountability requirements.

## Practicality and Feasibility

This architecture is practical because it uses widely deployable controls:
- API gateways and policy engines
- Token vault services
- IAM enforcement and logging
- Automated retention and deletion workflows

It significantly reduces tracking risk, but complete anonymity cannot be guaranteed against advanced cross-context tracking.

## Conclusion

The privacy-friendly architecture reduces tracking and identity correlation by combining anonymization, tokenization, minimal data collection, and compliance controls. It is practical for a global company because it balances user privacy, operational needs, and regulatory requirements.
