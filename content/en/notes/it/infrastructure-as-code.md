---
title: Infrastructure as Code
linktitle: Infrastructure as Code
toc: true
type: book

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 2


---

## Blog Posts by Gruntwork about terraform

If you just start using terraform these are really handy resources written in the grundwork blog:

1. [A Comprehensive Guide to Terraform](https://blog.gruntwork.io/a-comprehensive-guide-to-terraform-b3d32832baca)
2. [Why we use Terraform and not Chef, Puppet, Ansible, SaltStack, or CloudFormation](https://blog.gruntwork.io/why-we-use-terraform-and-not-chef-puppet-ansible-saltstack-or-cloudformation-7989dad2865c)
3. [An Introduction to Terraform](https://blog.gruntwork.io/an-introduction-to-terraform-f17df9c6d180)
4. [How to manage Terraform state](https://blog.gruntwork.io/how-to-manage-terraform-state-28f5697e68fa)
5. [How to create reusable infrastructure with Terraform modules](https://blog.gruntwork.io/how-to-create-reusable-infrastructure-with-terraform-modules-25526d65f73d)
6. [Terraform tips & tricks: loops, if-statements, and gotchas](https://blog.gruntwork.io/terraform-tips-tricks-loops-if-statements-and-gotchas-f739bbae55f9)
7. [How to use Terraform as a team](https://blog.gruntwork.io/how-to-use-terraform-as-a-team-251bc1104973)

## Terraform Book

If you do terraform, read it.
It is a quick and good read and you will save a lot of time doing common mistakes.

Out of these blog post Yevgeniy Brikman has created the book "[Terraform Up & Running](https://www.terraformupandrunning.com/)".

## Terraform Remote State

HashiCorp offers a free remote state for small teams and personal projects.

[Terraform Cloud plans](https://www.hashicorp.com/products/terraform/pricing)

[Login to Terraform cloud for your remote state](https://app.terraform.io/)

## Infrastructure as Code in one video

Damn good talk about Infrastructure as Code and the state of it.

[5 Lessons Learned From Writing Over 300,000 Lines of Infrastructure Code](https://www.youtube.com/watch?v=RTEgE2lcyk4)

{{< youtube RTEgE2lcyk4 >}}

## Keep your terraform configuration DRY

When using multiple states you can create the remote backend dynamically with this tool.
Less copy paste for remote backend setup.

[Terragrunt Website](https://terragrunt.gruntwork.io/)

[Terragrunt git repo](https://github.com/gruntwork-io/terragrunt)

## Using multiple terraform version on your machine

[tfenv](https://github.com/tfutils/tfenv)

## How to test terraform modules

[terratest](https://github.com/gruntwork-io/terratest)


## Production Readiness Checklist - Everything you need to do to go live

Production Readiness Checklist
It is not to specific to AWS

[Production Readiness Checklist](https://gruntwork.io/devops-checklist/)
