# NGINX Ingress Controller Installation Guide

This guide provides step-by-step instructions for installing the NGINX Ingress Controller in a Kubernetes environment.

## Table of Contents

- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Installation Steps](#installation-steps)
  - [Add the NGINX Ingress Helm Repository](#add-the-nginx-ingress-helm-repository)
  - [Install the NGINX Ingress Controller](#install-the-nginx-ingress-controller)
  - [Verify the Installation](#verify-the-installation)
  - [Deploy a Test Application](#deploy-a-test-application)
  - [Create an Ingress Resource](#create-an-ingress-resource)
  - [Access the Application](#access-the-application)
- [Conclusion](#conclusion)
- [Additional Resources](#additional-resources)

## Introduction

The NGINX Ingress Controller is a popular choice for managing external access to HTTP/HTTPS services in a Kubernetes cluster. This guide covers the installation and basic testing of the controller.

## Prerequisites

- A Kubernetes cluster
- `kubectl` installed and configured
- Helm 3 installed (optional, but recommended)

## Installation Steps

### Add the NGINX Ingress Helm Repository

If using Helm, add the NGINX repository:

```bash
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
