# Mediroza-General-Hospital-Penetration-Testing-Report


![Assessment](https://img.shields.io/badge/Assessment-Penetration%20Testing-blue)
![Networkwalks](https://img.shields.io/badge/Networkwalks-Week%204-orange)
![Batch](https://img.shields.io/badge/Batch-B084-informational)
![Risk](https://img.shields.io/badge/Overall%20Risk-CRITICAL-red)

---

## 1. Project Information

| Attribute | Details |
|---|---|
| Project | Mediroza General Hospital Penetration Testing |
| Target | https://medirozahospital.com |
| Assessment Type | Black-Box Web Application Penetration Test |
| Assessment Date | 24 November 2026 |
| Duration | 5 Days |
| Training Program | Networkwalks – Week 4 |
| Batch | B084 |
| Security Analyst | Samweli Jacobo Kinanda |
| Overall Risk Rating | CRITICAL |

---

## 2. Executive Summary

This project documents a black-box penetration testing assessment of the Mediroza General Hospital web application as part of the Networkwalks Week 4 practical cybersecurity training.

The primary objective was to evaluate the security of the target web application, identify potential vulnerabilities, validate security weaknesses through controlled testing, and document their potential impact.

The assessment exercise identified three major security weaknesses:

1. SQL Injection resulting in authentication bypass in the Patient Portal.
2. Weak PDF encryption exposing confidential medical information to offline password attacks.
3. Publicly accessible database backup files exposing sensitive Human Resources and shareholder information.

The Staff Portal was also evaluated and resisted the tested authentication bypass attempts.

The assessment demonstrated how multiple security weaknesses can be combined into an attack chain, potentially resulting in unauthorized access to sensitive organizational information.

**Overall Assessment: CRITICAL**

---

## 3. Scope of Assessment

### 3.1 Target

- Primary target: `https://medirozahospital.com`
- Application components: Patient Portal and Staff Portal.
- Supporting resources: Publicly accessible directories, PDF documents, and backup files.

### 3.2 Testing Scope

The assessment focused on:

- Web application reconnaissance.
- Authentication and access control testing.
- SQL Injection identification and validation.
- PDF encryption security analysis.
- Metadata inspection.
- Directory and file enumeration.
- Sensitive information exposure.
- Security recommendations and remediation.

### 3.3 Out-of-Scope Activities

- Social engineering.
- Denial-of-Service attacks.
- Destructive testing.
- Unauthorized modification of production records.

All testing must remain within the explicitly authorized training environment and agreed scope.

---

## 4. Tools and Technologies

| Tool | Purpose |
|---|---|
| Kali Linux | Penetration testing environment |
| Burp Suite | Web application traffic interception and analysis |
| Gobuster | Directory and file enumeration |
| ExifTool | PDF metadata inspection |
| pdf2john | PDF password hash extraction |
| John the Ripper | Password security testing |
| PDFCrack | PDF password recovery testing |
| qpdf | PDF encryption and document inspection |
| SQL Analysis | SQL Injection identification and validation |

---

## 5. Methodology

The assessment followed a structured penetration testing workflow informed by OWASP Web Application Security Testing principles.

The major phases included:

1. **Reconnaissance:** Understanding the target application's publicly accessible components.
2. **Enumeration:** Identifying accessible directories, files, and application endpoints.
3. **Vulnerability Assessment:** Identifying weaknesses in authentication, encryption, and access control.
4. **Exploitation:** Validating identified vulnerabilities within the authorized environment.
5. **Post-Exploitation Analysis:** Assessing the potential exposure of sensitive information.
6. **Reporting:** Documenting findings, evidence, severity, and remediation recommendations.

---

# 6. Technical Findings and Evidence

## Finding 01: SQL Injection – Patient Portal Authentication Bypass

**Severity:** CRITICAL

**Affected Component:** Patient Portal Login

**Vulnerability Category:** Injection – SQL Injection

### Description

The Patient Portal authentication mechanism was vulnerable to SQL Injection because user-supplied input was not properly handled within the database query.

### Exploitation Summary

During the practical assessment, the following test input was used in the username field:

`admin' -- -`

The input manipulated the SQL query structure and bypassed the normal password verification process in the tested environment.

### Security Impact

Successful exploitation could allow an unauthenticated attacker to:

- Bypass the normal authentication process.
- Access restricted Patient Portal functionality.
- View or download patient information.
- Compromise the confidentiality of medical records.

### Evidence and Screenshots

**Screenshot 01: Patient Portal Login Interface**

<img width="938" height="433" alt="patient portal" src="https://github.com/user-attachments/assets/7e9b097e-ba85-46b9-98ed-d5e51901feea" />


*Description: Initial Patient Portal login interface observed during reconnaissance.*

**Screenshot 02: SQL Injection Testing**

<img width="934" height="437" alt="sql injection" src="https://github.com/user-attachments/assets/b6fbb7cd-b4ac-443f-8eb2-f4a453e25679" />


*Description: Captured request and SQL Injection testing using Burp Suite.*

**Screenshot 03: Authentication Bypass Validation**

<img width="937" height="429" alt="Screenshot 2026-11-23 202214" src="https://github.com/user-attachments/assets/90a6956f-75b7-4283-9e36-872c2e670970" />


*Description: Evidence demonstrating successful authentication bypass in the tested environment.*

### Remediation

- Implement parameterized SQL queries or prepared statements.
- Avoid constructing database queries through string concatenation.
- Apply server-side input validation.
- Enforce proper authentication and authorization checks.
- Use least-privilege database accounts.
- Perform regular application security testing.

---

## Finding 02: Weak PDF Encryption

**Severity:** HIGH

**Affected Component:** Patient Laboratory Reports

**Vulnerability Category:** Cryptographic Failures

### Description

The downloaded PDF laboratory reports were protected using weak passwords that could be recovered through offline dictionary-based password testing.

### Exploitation Summary

The assessment involved:

1. Obtaining PDF documents through the tested application.
2. Extracting password hashes using `pdf2john`.
3. Performing dictionary-based password recovery using John the Ripper and PDFCrack.
4. Using `qpdf` to inspect and decrypt documents after successful password recovery.

The exercise demonstrated that weak document passwords do not provide adequate protection for confidential medical information.

### Security Impact

- Unauthorized access to medical documents.
- Exposure of sensitive patient information.
- Increased risk of identity theft and privacy violations.
- Potential regulatory and reputational consequences.

### Evidence and Screenshots

**Screenshot 04: PDF Encryption Inspection**

<img width="894" height="419" alt="cracked pdf one" src="https://github.com/user-attachments/assets/10736f11-0c87-4d5f-b0c4-daabc8fb47ca" />


*Description: Inspection of PDF encryption and document protection.*

**Screenshot 05: Password Recovery Testing**

<img width="727" height="405" alt="open pdf one" src="https://github.com/user-attachments/assets/95847fcd-9a00-482b-aa04-7a69eef94be5" />


*Description: Password hash extraction and offline dictionary testing.*

**Screenshot 06: PDF Decryption Validation**

<img width="818" height="422" alt="cracked pdf two" src="https://github.com/user-attachments/assets/4338ffad-1b15-4fc3-a4eb-b165d5e98088" />


*Description: Validation of document decryption after successful password recovery. Use sanitized evidence only.*

### Remediation

- Use modern, appropriately configured encryption.
- Generate strong, unique document passwords.
- Avoid dictionary words and predictable passwords.
- Implement secure document access controls.
- Use secure key management practices.
- Do not treat PDF password protection as a replacement for application-level authorization.

---

## Finding 03: Exposed Database Backup and Sensitive Information Disclosure

**Severity:** CRITICAL

**Affected Component:** Publicly Accessible Server Files

**Vulnerability Category:** Security Misconfiguration / Sensitive Data Exposure

### Description

A legacy database backup was identified as publicly accessible during the assessment exercise.

The backup file contained sensitive information associated with the organization's staff and shareholder records.

### Exploitation Summary

The investigation involved:

1. Inspecting PDF document metadata using ExifTool.
2. Identifying potentially sensitive internal path information.
3. Performing directory enumeration using Gobuster.
4. Identifying the legacy database backup file:

`mediroza_db_backup_2019.sql`
<img width="935" height="419" alt="sql backup" src="https://github.com/user-attachments/assets/a61578c8-d0d1-4de1-8966-b87438e9443c" />


5. Analyzing the backup structure and identifying sensitive database tables, including staff and shareholder records.

### Security Impact

The exposed backup potentially enabled unauthorized access to:

- Employee personal information.
- Employee salary records.
- Contact information.
- National identification information.
- Shareholder and corporate ownership information.

This represents a significant confidentiality risk.

### Evidence and Screenshots

**Screenshot 07: PDF Metadata Analysis**

![Metadata Analysis](evidence/07-metadata-analysis.png)

*Description: Metadata inspection using ExifTool.*

**Screenshot 08: Directory Enumeration**

![Gobuster Enumeration](evidence/08-gobuster-enumeration.png)

*Description: Directory and file enumeration using Gobuster.*

**Screenshot 09: Database Backup Discovery**

![Backup Discovery](evidence/09-backup-discovery.png)

*Description: Identification of the legacy database backup file.*

**Screenshot 10: Database Structure Analysis**

![Database Analysis](evidence/10-database-analysis.png)

*Description: Redacted database structure showing the presence of sensitive tables.*

### Remediation

- Immediately remove exposed backup files from public web directories.
- Store backups outside the web root.
- Restrict backup access through appropriate permissions and access controls.
- Encrypt database backups.
- Implement automated backup security reviews.
- Disable unnecessary directory listing.
- Monitor web server access logs for suspicious backup-file requests.

---

## Finding 04: Staff Portal Authentication Testing

**Severity:** INFORMATIONAL – No successful bypass observed

**Affected Component:** Staff Portal Login

### Description

The Staff Portal authentication interface was evaluated using selected SQL Injection test inputs and credentials identified during the training exercise.

The tested attempts did not successfully bypass the Staff Portal authentication mechanism.

This observation suggests that the tested login functionality may have stronger input handling or authentication controls than the Patient Portal.

However, this does not establish that the entire Staff Portal is secure.

### Evidence and Screenshot

**Screenshot 11: Staff Portal Authentication Testing**

![Staff Portal Testing](evidence/11-staff-portal-testing.png)

*Description: Authentication testing showing the observed response to the selected test inputs.*

### Security Recommendations

- Continue using parameterized SQL queries.
- Enforce Multi-Factor Authentication.
- Apply strong password storage mechanisms.
- Conduct periodic authentication and authorization testing.
- Review access controls independently of database backup security.

---

## 7. Risk Assessment Summary

| ID | Finding | Severity |
|---|---|---|
| F-01 | SQL Injection – Authentication Bypass | CRITICAL |
| F-02 | Weak PDF Encryption | HIGH |
| F-03 | Exposed Database Backup | CRITICAL |
| F-04 | Staff Portal Authentication Testing | INFORMATIONAL |

### Overall Risk: CRITICAL

The combination of authentication weaknesses, inadequate document protection, and publicly accessible backup files creates a significant risk of sensitive information exposure.

---

## 8. Recommended Remediation Priorities

| Priority | Recommendation | Urgency |
|---|---|---|
| 1 | Fix SQL Injection vulnerabilities | Immediate |
| 2 | Remove publicly accessible database backups | Immediate |
| 3 | Review and strengthen document encryption | High |
| 4 | Enforce MFA and strong authentication | High |
| 5 | Remove sensitive PDF metadata | Medium |
| 6 | Implement continuous security monitoring | Ongoing |

---

## 9. Lessons Learned

This practical exercise provided hands-on experience in several important cybersecurity areas:

- Understanding web application reconnaissance.
- Identifying SQL Injection vulnerabilities.
- Understanding authentication bypass techniques.
- Analyzing encrypted PDF documents.
- Performing password security assessments.
- Inspecting document metadata.
- Conducting directory and file enumeration.
- Identifying exposed database backups.
- Assessing sensitive information disclosure.
- Preparing professional penetration testing reports.

The exercise also demonstrated that organizational security cannot depend on a single protective mechanism. Security must be implemented across the entire application, infrastructure, and data management lifecycle.

---

## 10. Conclusion

The Mediroza General Hospital penetration testing exercise demonstrated how weaknesses in web application authentication, document encryption, and backup management can create serious security risks.

The findings emphasize the importance of secure software development practices, appropriate encryption, proper access control, and secure infrastructure configuration.

Organizations handling sensitive information should conduct regular security assessments and promptly address identified vulnerabilities.

**The most important lesson:** A single exposed vulnerability can become the starting point for a much larger security compromise when multiple weaknesses exist within the same environment.

## 12. Author Information

**Name:** Samweli Jacobo Kinanda

**Training Program:** Networkwalks Cybersecurity Training

**Batch:** B084

**Project:** Week 4 – Penetration Testing

**Area of Interest:** Cybersecurity | Penetration Testing | SOC Analysis

---

## Disclaimer

This repository documents a cybersecurity training exercise for educational purposes.

All penetration testing activities must be conducted only against systems for which explicit authorization has been obtained.

The techniques documented here must not be used to access, modify, or disclose information belonging to unauthorized systems or individuals.

Do not publish real patient information, passwords, National IDs, employee salary records, or confidential database contents.

**Project Status:** Training Assessment Documentation

**Report Date:** 24 November 2026
