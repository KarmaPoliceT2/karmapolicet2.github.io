# RocketPool Node Install

## Background

This documentation is the process used to setup a RocketPool node with 2x validators on it.

## Conventions

- Items inside <> characters are to be substituted with your own values
- Items with :exclamation: have an alternate step for implementing in TestNet
  - The alternate step will be marked as a markdown quote such as this:
       > This is an alternate step taken for TestNet setup

## Starting Assumptions

- You have a system built with no operating system on it, and on which only your node will run.
  - My system:
    | Component | Description | Cost | Date |
    | --------- | ----------- | ---- | ----- |
    | NUC | Intel NUC12WSHi7 | $645.00 | 2023-04-20 |
    | RAM | Crucial 32GB (2x16GB) DDR4 3200MHz CL22 | $68.99 | 2023-04-20 |
    | SSD | Samsung 990 PRO 2TB PCIe 4.0 m.2 | $179.99 | 2023-04-20 |
    | Hardware Wallet | Ledger Nano S Plus | ?? | ?? |
    | Seed Storage | Cryptotag Zeus | ?? | ?? |

## Table of Contents

 1. OS Installation
 2. OS Hardening
 3. Wallets, Seeds & Addresses
