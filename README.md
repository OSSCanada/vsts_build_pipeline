# VSTS Build Pipeline on Azure Workshop

## Overview

This workshop will guide you through building a CI/CD pipeline with VSTS for use with Azure Container Service (AKS - Managed Kubernetes Service) on Azure.

The labs are based upon a node.js application that allows for voting on the Justice League Superheroes. Data is stored in MongoDB backend.

> Note: These labs are designed to run on a Linux CentOS VM running in Azure (jumpbox) along with Azure Cloud Shell. They can potentially be run locally on a Mac or Windows, but the instructions are not written towards that experience. ie - "You're on your own."

> Note: Since we are working on a jumpbox, note that Copy and Paste are a bit different when working in the terminal. You can use Shift+Ctrl+C for Copy and Shift+Ctrl+V for Paste when working in the terminal. Outside of the terminal Copy and Paste behaves as expected using Ctrl+C and Ctrl+V. 

## Prerequisite Lab

You must have completed one (1) of the following Hands on Labs/"Hackfests":
1. [Microsoft Intelligent Cloud Blackbelt Team :: Hackfest](https://github.com/Azure/blackbelt-aks-hackfest): Labs 1-4
  - This HoL/hackfest is a detailed guide to deploying apps to a Kuberenetes istributed computing cluster on Azure (i.e. AKS)
2. [OSS Canada AKS Mini Hackfest](https://github.com/OSSCanada/aks-mini-hackfest)
  - This HoL/hackfest is a subset of the [Microsoft Intelligent Cloud Blackbelt Team :: Hackfest](https://github.com/Azure/blackbelt-aks-hackfest).  It only includes steps to build apps into docker images, deploying those images to a container registry and creating an Azure Managed Kubernetes Service (i.e. AKS).

## Hands-on Lab Guide

- Complete one (1) of the [Prerequisite Labs](#prerequisite-lab) first

1. [Setting up VSTS](hol-content/01-setup_vsts.md)
2. [Building a VSTS Continuous Integration Pipeline](hol-content/02-build_vsts_ci.md)
3. [Building a VSTS Continuous Deployment Pipeline](hol-content/03-build_vsts_cd.md)
