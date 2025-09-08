#reporting [Sample Pentest Report](https://github.com/hmaverickadams/TCM-Security-Sample-Pentest-Report)
### Report Outline
1. Title Page
2. Table of Contents
3. Disclaimers
4. Contact Info
5. Assessment Overview
	1. Methodology
	2. Assessment Components
		1. Checklists
		2. Tools used
	3. CVSS Severity Scoring Definitions
	4. Scope
6. Executive Summary
	1. Key Strengths and Weaknesses
	2. Vulnerability Summary
7. Technical Summary
	1. Each vulnerability ordered by criticality
		1. Description (with severity)
		2. Risk
			1. Likelihood
			2. Impact
		3. System/Host
		4. Tools Used
		5. Resources
	2. Evidence
	3. Steps to Reproduce
	4. Remediation Advice
8. Additional Scans and Reports
	1. Burp Suite files
	2. Vulnerability scans
### Best Practices
Justify text
Double-check grammar, punctuation, spacing
Consistency across the paper

`nmap -p 443 --script=ssl-enum-ciphers`