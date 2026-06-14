---
title: 'CONFIGURED'
date: '2026-06-14T08:20:00+08:00'
draft: false
summary: 'A C++17 and Qt 6 desktop application for creating, validating, exporting, and Git-managing structured robotics configuration projects.'
tags: ['qt', 'cpp', 'robotics', 'configuration', 'git', 'desktop-app']
---

# CONFIGURED

CONFIGURED is a desktop application I have been building to make robotics configuration work less fragile. The goal is to give robotics projects a structured place to define systems, subsystems, components, and parameters without relying on hand-edited JSON, XML, spreadsheets, or loose notes.

The app is written in C++17 with Qt 6 and is aimed at the kind of configuration work that appears once a robot project grows past a few launch files: platform metadata, parameter lists, Git-backed project folders, exportable configuration files, and repeatable project setup.

## What It Does

CONFIGURED lets me build a configuration tree such as:

    System
      Subsystem
        Component
          Parameter

Each parameter can carry a key, value, unit, required flag, and validation state. Projects are saved as .configured files and can be exported to JSON or XML for downstream tooling.

Current features include:

- Creating and opening structured configuration projects
- Editing system, subsystem, component, and parameter nodes
- Inline validation for project names, item names, required fields, and duplicate parameter keys
- Git-managed project workflows for status, commit, pull, push, branch switching, and remote connection
- JSON and XML parameter export
- Windows installer bundle packaging
- Ubuntu .deb package generation
- Read the Docs documentation with Doxygen-generated C++ API references

## Why I Built It

Robotics projects accumulate configuration quickly. A real robot can involve firmware settings, control parameters, sensor details, platform metadata, simulation settings, and export formats for other tools. Those values often end up spread across files and notes.

I wanted a tool that treats configuration as a first-class project artifact: something visible, validated, versioned, and exportable.

CONFIGURED also became a useful engineering exercise in building a release-ready desktop tool rather than only a prototype. A lot of the recent work has been about the unglamorous parts that make software easier to trust:

- Separating application workflows into services and controllers
- Adding validation layers and focused tests
- Improving Git workflow handling
- Packaging for Windows and Ubuntu
- Setting up documentation and release notes
- Fixing Qt compatibility issues between Windows and Linux builds

## Recent Release Work

The first public release pass focused on making CONFIGURED easier to build, package, and understand.

I added Windows installer packaging, Ubuntu .deb packaging, Linux build presets, Read the Docs documentation, Doxygen API output, a Google-based clang-format setup, and more automated tests around project persistence and validation.

One of the current feature additions is Save As, which copies a project into its own project folder and treats the copy as a standalone project rather than carrying over the original .git repository.

## Links

- [GitHub repository](https://github.com/reeceholland/configured)
- [Read the Docs](https://configured.readthedocs.io/)
