---
layout: post
title: "Automatically Set Environment"
date: 2026-02-25 00:00:00 +0900
author: kang
categories: [Develop, Environment]
tags: [Develop, Environment, bat]
pin: false
math: true
mermaid: true
---

# Bat File
---
```bat
@echo off
REM ===============================
REM 1. Brower
REM ===============================
start chrome "https://chat.openai.com"
start chrome "https://google.com"
start chrome "https://kcnote.github.io/"
start chrome "https://github.com/KCnote/KCnote.github.io/"

REM ===============================
REM 2. VS Code
REM ===============================
start "" "code"

REM ===============================
REM 3. Git Bash & move directory
REM ===============================
start "" "C:\Program Files\Git\git-bash.exe" --cd="C:/Users/Desktop/Github"
```