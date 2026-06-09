# HTB - Meow Machine Writeup

## 🖥️ Target Info
- Machine Name: Meow
- Difficulty: Very Easy
- OS: Linux
- Focus: Basic enumeration, Telnet access

---

## 🔍 Enumeration

I began with an Nmap scan to identify open ports and services:

```bash
nmap -sV -sC <target-ip>
