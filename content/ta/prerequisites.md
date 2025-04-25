---
title: "Quickstart Guide"
description: "A step-by-step guide to get started with Test Assistant (TA)."
date: 2025-04-23
weight: 20
draft: false
---

# Quickstart Guide

## Overview

Test Assistant (TA) offers **Starter Packs** to simplify your setup process. However, if you prefer to configure everything manually, refer to the [General Configuration](#general-configuration) section below.

## Prerequisites

### Kubernetes and Helm Requirements

- **Kubernetes** version 1.28 or higher  
  - `ingress-nginx`
  - `cert-manager`
  - PostgreSQL with TA DB Pack
  - Ocular Framework (if used for authentication)

- **Helm** version 3.0 or higher

### System Requirements

| Component | Minimum Requirement |
|----------|---------------------|
| CPU      | 2 vCPUs             |
| Memory   | 8 GB                |
| Storage  | 50 GB               |

### Prerequisite Packages

TA provides two packages:
- **Bundles**
- **Configuration Migration Assistant (CMA)**

Deploy **one** of these before installing TA to ensure it works correctly.

### Selenium IDE Plugin

Install the Selenium IDE browser plugin to record UI test scenarios.  
More info: [Selenium IDE](https://www.selenium.dev/selenium-ide/)

## Starter Packs

Starter Packs include pre-defined test scenarios across industries:

- **Banking**
- **Healthcare**
- **Utilities**

### Use a Starter Pack

To use a starter pack:

1. Install Test Assistant.
2. Meet all [Prerequisites](#prerequisites).
3. Complete [Required Actions](#required-test-automation-actions).
4. Modify your test data ([Datasets](/ta/3_0_0/docs/web-application/dataset/)).
5. (Optional) Update payload details in the **Services** module for custom APIs.
6. Run the flow via **Flow** module.
7. View results in the **Results** section.

### Required Test Automation Actions

- Ensure your **environment configurations** and **authentication details** are correct.
- Set credentials and DB access for the configured environment.

## General Configuration

If you want to configure everything manually:

1. Meet all prerequisites.
2. Install Test Assistant.
3. Create a **Project**.
4. Create a **Step** for that project.
5. Create an **Assertion**.
6. Create a **Dataset**.
7. Run a **Test Flow**.
8. View **Test Results**.

---
