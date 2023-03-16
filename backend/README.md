# Hack Club Bank Backend

The goal of this directory is to provide some insight into how Hack Club Bank
works from an engineering perspective.

## Summary

- Hack Club Bank is built with [Ruby on Rails](https://rubyonrails.org/).
- The [`models.md`](models.md) file contains a list of all the critical
user-facing models used by Bank.
- There currently exists a [Transparency
API](https://bank.hackclub.com/docs/api/v3/) that exposes some of Hack Club
Bank's data. However, it is read-only and only supports Organizations with
transparency mode on.
