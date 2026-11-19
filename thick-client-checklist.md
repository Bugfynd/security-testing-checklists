# Thick Client Penetration Testing Checklist

A practical checklist we use when testing Windows desktop (thick client) applications — client side, network traffic and server side.

**90 checks across 9 areas.** Tick each box as you test.

## Contents

1. [Information Gathering](#1-information-gathering)
2. [GUI Testing](#2-gui-testing)
3. [File Testing](#3-file-testing)
4. [Registry Testing](#4-registry-testing)
5. [Network Testing](#5-network-testing)
6. [Assembly Testing](#6-assembly-testing)
7. [Memory Testing](#7-memory-testing)
8. [Traffic Testing](#8-traffic-testing)
9. [Common Vulnerabilities](#9-common-vulnerabilities)

## 1. Information Gathering

### Application profiling

- [ ] **IG-01** Identify the application architecture (two-tier or three-tier)
- [ ] **IG-02** Identify the technologies used (languages, frameworks, runtimes)
- [ ] **IG-03** Identify network communication (protocols, ports, endpoints)
- [ ] **IG-04** Observe the application process and child processes
- [ ] **IG-05** Observe each functionality and behaviour of the application
- [ ] **IG-06** Identify all entry points (UI, files, network, IPC, command line)
- [ ] **IG-07** Analyze the security mechanisms (authentication and authorization)
- [ ] **IG-08** Identify third-party libraries and their versions
- [ ] **IG-09** Review the installer and auto-update mechanism

**Tools:** CFF Explorer · Sysinternals Suite · Wireshark · PEiD · Detect It Easy (DIE) · Strings

## 2. GUI Testing

### GUI object permissions

- [ ] **GUI-01** Display hidden form objects
- [ ] **GUI-02** Try to activate disabled functionalities
- [ ] **GUI-03** Try to uncover masked passwords

### GUI content

- [ ] **GUI-04** Look for sensitive information shown in the interface

### GUI logic

- [ ] **GUI-05** Test for access control and injection-based vulnerabilities
- [ ] **GUI-06** Bypass controls by using intended GUI functionality
- [ ] **GUI-07** Check for improper error handling
- [ ] **GUI-08** Check for weak input sanitization
- [ ] **GUI-09** Try privilege escalation (unlocking admin features for normal users)
- [ ] **GUI-10** Try payment or price manipulation
- [ ] **GUI-11** Check clipboard handling of sensitive fields

**Tools:** UISpy · WinSpy++ · Window Detective · Snoop WPF

## 3. File Testing

### File permissions

- [ ] **FILE-01** Check permissions on every application file and folder

### File integrity

- [ ] **FILE-02** Check strong naming
- [ ] **FILE-03** Verify code signing (Authenticode)

### File content

- [ ] **FILE-04** Look for sensitive information on the file system (symbols, data, passwords, configurations)
- [ ] **FILE-05** Look for sensitive information in configuration files
- [ ] **FILE-06** Look for hard-coded encryption keys or data
- [ ] **FILE-07** Look for clear-text storage of sensitive data
- [ ] **FILE-08** Look for side-channel data leakage
- [ ] **FILE-09** Check logs for sensitive or unreliable logging
- [ ] **FILE-10** Check temporary and cache files left after use

### File and content manipulation

- [ ] **FILE-11** Try framework backdooring
- [ ] **FILE-12** Try DLL preloading
- [ ] **FILE-13** Perform race condition checks
- [ ] **FILE-14** Test file and content replacement
- [ ] **FILE-15** Test client-side protection bypass using reverse engineering

### Exported functions

- [ ] **FILE-16** Identify exported functions
- [ ] **FILE-17** Try to use exported functions without authentication

### Public methods

- [ ] **FILE-18** Build a wrapper to reach public methods without authentication

### Decompile and rebuild

- [ ] **FILE-19** Try to recover original source code, passwords and keys
- [ ] **FILE-20** Try to decompile the application
- [ ] **FILE-21** Try to rebuild the application
- [ ] **FILE-22** Try to patch the application

### Decryption and de-obfuscation

- [ ] **FILE-23** Try to recover original source code
- [ ] **FILE-24** Try to retrieve passwords and keys
- [ ] **FILE-25** Test for lack of obfuscation

### Disassemble and reassemble

- [ ] **FILE-26** Try to build a patched assembly

**Tools:** Strings · dnSpy / ILSpy · Procmon · Process Explorer · Process Hacker

## 4. Registry Testing

### Registry permissions

- [ ] **REG-01** Check read access to the application's registry keys
- [ ] **REG-02** Check write access to the application's registry keys

### Registry contents

- [ ] **REG-03** Inspect registry contents
- [ ] **REG-04** Check for sensitive information stored in the registry
- [ ] **REG-05** Compare the registry before and after running the application

### Registry manipulation

- [ ] **REG-06** Try registry manipulation
- [ ] **REG-07** Try to bypass authentication by registry manipulation
- [ ] **REG-08** Try to bypass authorization by registry manipulation

**Tools:** Regshot · Procmon · AccessEnum

## 5. Network Testing

### Network

- [ ] **NET-01** Check for sensitive data in transit
- [ ] **NET-02** Try to bypass firewall rules
- [ ] **NET-03** Try to manipulate network traffic
- [ ] **NET-04** Check TLS configuration and certificate validation

**Tools:** Wireshark · TCPView

## 6. Assembly Testing

### Binary protections

- [ ] **ASM-01** Verify Address Space Layout Randomization (ASLR)
- [ ] **ASM-02** Verify SafeSEH
- [ ] **ASM-03** Verify Data Execution Prevention (DEP)
- [ ] **ASM-04** Verify strong naming
- [ ] **ASM-05** Verify Control Flow Guard (CFG)
- [ ] **ASM-06** Verify HighEntropyVA

**Tools:** PESecurity · Binskim

## 7. Memory Testing

### Memory content

- [ ] **MEM-01** Check for sensitive data stored in memory
- [ ] **MEM-02** Check that secrets are cleared from memory after use

### Memory manipulation

- [ ] **MEM-03** Try memory manipulation
- [ ] **MEM-04** Try to bypass authentication by memory manipulation
- [ ] **MEM-05** Try to bypass authorization by memory manipulation

### Run-time manipulation

- [ ] **MEM-06** Analyze the process dump file
- [ ] **MEM-07** Check for process replacement
- [ ] **MEM-08** Check for assembly modification in memory
- [ ] **MEM-09** Try to debug the application
- [ ] **MEM-10** Identify dangerous functions
- [ ] **MEM-11** Use breakpoints to test each functionality

**Tools:** Process Hacker · HxD · Strings · x64dbg

## 8. Traffic Testing

### Traffic

- [ ] **TRF-01** Analyze the flow of network traffic
- [ ] **TRF-02** Look for sensitive data in transit
- [ ] **TRF-03** Intercept and modify requests to the server (non-HTTP and HTTP)

**Tools:** Echo Mirage · MITM Relay · Burp Suite · Fiddler

## 9. Common Vulnerabilities

### Common vulnerabilities

- [ ] **VULN-01** Try to decompile the application
- [ ] **VULN-02** Try reverse engineering
- [ ] **VULN-03** Test against the OWASP Top 10 (web)
- [ ] **VULN-04** Test against the OWASP API Security Top 10
- [ ] **VULN-05** Test for DLL hijacking
- [ ] **VULN-06** Test signature checks (Sigcheck)
- [ ] **VULN-07** Perform binary analysis
- [ ] **VULN-08** Test for business logic errors
- [ ] **VULN-09** Test for TCP/UDP attacks
- [ ] **VULN-10** Run automated code scanning (Visual Code Grepper)
- [ ] **VULN-11** Test insecure update mechanisms
- [ ] **VULN-12** Test insecure IPC (named pipes, sockets, COM)

---

*Base checklist adapted from the "Thick Client Pentesting Checklist" by Hari Prasanth ([hariprasanth.blogspot.com](https://hariprasanth.blogspot.com)). Restructured and expanded by Bugfynd.*

---

*Bugfynd · Cyber Security Testing & Training · contact@bugfynd.com*
