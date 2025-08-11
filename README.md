# Jenkins Pipeline Review

## Overview
This Jenkins pipeline automates deployment of the same web application to **Apache** and **Nginx** containers **in parallel**.

## Purpose
- Create a shared application directory.
- Remove existing containers.
- Run **Apache** and **Nginx** containers with volume mapping to the shared directory.
- Copy application files into the shared directory.
- Handle success/failure with post-build actions.

## Strengths
- **Parallel execution** for faster deployment.
- **Cleanup steps** for both directory and containers ensure a fresh start.
- **Post actions** to notify success or failure and clean up the workspace.
- **Volume mapping** for instant content sharing between host and containers.

## Suggested Improvements
- Use `try/catch` or `sh returnStatus:true` to prevent failures if a container doesn’t exist during removal.
- Parameterize **ports**, **container names**, and **application path** for flexibility.
- Add **health checks** after container creation to confirm they are running.
- Fix log message typo: Nginx stage currently says _"Creating the Apache container"_.

---
**Summary:**  
A solid parallel deployment pipeline for quick multi-webserver setups, but could be improved with better error handling, parameterization, and minor corrections.
