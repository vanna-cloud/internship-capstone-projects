# Security Policy



## Reporting a vulnerability

If you find a vulnerability in AlertDesk, report it privately to the
maintainer:

 **Name:** George
 **Email:** baningeorge@gmail.com

Do not publicly disclose vulnerabilities or include sensitive information
in public issues.

## Authentication (scaffold vs Month 4)


The bearer tokens and their associated roles are defined in `config/client-spec.json`.

These committed bearer tokens are predictable scaffold credentials for testing only. **Never use them outside the internship lab or in production.** They must not be treated as secure credentials or deployed as usable authentication secrets.

Before any production deployment, replace the stub authentication with a proper authentication mechanism. Proper authentication is planned for Month 4.



## Database and audit log

AlertDesk stores ticket data in a SQLite database.

The database location is controlled by the `ALERTDESK_DB` environment variable. If this variable is not set, the application uses `data/alertdesk.sqlite`.

Security-relevant actions are recorded in the `audit_events` table. Ticket creation, assignment, and status transitions are audited.

SQLite `BEFORE UPDATE` and `BEFORE DELETE` triggers prevent existing audit records from being modified or deleted through normal database operations. Audit records are therefore append-only under the application's normal database operation. These triggers do not protect against an administrator with direct access to the database file who can alter the schema.

Audit logs must not contain passwords, bearer tokens, or other sensitive secrets.

Database backups should be performed regularly according to the deployment environment's backup and recovery requirements. Backups should be stored securely and tested periodically.

## Scope limitations

This security policy applies to the AlertDesk internship/scaffold
environment.

AlertDesk is **not a SOAR (Security Orchestration, Automation and
Response) platform or a SIEM (Security Information and Event Management)
platform**.

The scaffold uses stub tokens for testing only and should not be used
for real security incidents or production security operations.

The following are outside the current scaffold scope:

 Slack integration
 SIEM connectors
 SSO
 Multi-region operation 

### Known security limitations

The current internship scaffold has the following security limitations:

- Authentication uses predictable stub bearer tokens and is not suitable for production.
- Authorization is role-based and limited to the roles and permissions defined in the client specification.
- The application does not currently provide production-grade identity management or single sign-on.
- Audit records are protected against normal SQL updates and deletes through SQLite triggers, but this does not protect against direct database-file or schema manipulation by a privileged user.
- Production-grade monitoring, alerting, backup management, and recovery procedures must be established before production deployment.

These limitations should be reviewed and addressed before AlertDesk is used in a production security environment. This list is not a complete OWASP compliance assessment.
