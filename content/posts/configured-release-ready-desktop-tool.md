---
title: 'Taking CONFIGURED Toward a Release'
date: '2026-06-15T08:50:00+08:00'
draft: false
tags: ['qt', 'cpp', 'robotics', 'configuration', 'desktop-app', 'release']
categories: ['Development Log']
summary: 'A development update on CONFIGURED: a Qt/C++ robotics configuration tool, now packaged for Windows and Ubuntu with documentation, validation, Git workflows, and release assets.'
---

# Taking CONFIGURED Toward a Release

I have been spending time on CONFIGURED, a desktop application for creating and managing
structured robotics configuration projects. It started as a practical tool idea: I wanted a
cleaner way to describe robot systems, subsystems, components, and parameters without
spreading configuration across hand-edited files and notes.

The application is written in C++17 with Qt 6. It saves projects as .configured files and
can export parameter data to JSON or XML for downstream tooling.

## What CONFIGURED Is For

Robotics projects collect configuration quickly. There are platform details, sensor
settings, controller parameters, units, required values, metadata, exports, Git state, and
the usual question of whether the file on disk is actually the file you meant to edit.

CONFIGURED is my attempt to make that work more deliberate. A project is represented as a
hierarchy:

    System
      Subsystem
        Component
          Parameter

Each parameter can carry a key, value, unit, required flag, and validation state. The goal
is not to replace every configuration format, but to provide a structured place to design,
check, version, and export configuration data.

## Recent Work

A lot of the recent effort has been release-readiness work rather than flashy feature work.
That has included:

- Project validation for metadata, item names, required fields, and duplicate parameter keys
- Git workflows for status, commits, pull, push, branch switching, and remote connection
- JSON and XML export paths
- Windows installer bundle generation
- Ubuntu .deb packaging
- A Linux desktop launcher for the installed package
- Read the Docs documentation
- Doxygen-generated C++ API docs published through Sphinx and Breathe
- A Google-based clang-format setup
- More focused tests around project persistence and validation

This kind of work is easy to underestimate. It is not the first thing people notice in an
application, but it is what makes a project easier to trust, reproduce, and hand to someone
else.

## Cross-Platform Lessons

The Windows and Ubuntu builds exposed the usual desktop application details. Windows needs a
bundle that carries the Qt runtime and installer pieces. Ubuntu can ship a much smaller .deb
because it can depend on system packages.

There were also Qt compatibility details to handle. One example was replacing a newer
QCheckBox signal with an older-compatible signal so the app builds cleanly against the Qt 6
packages available on Ubuntu.

The packaging work also forced useful questions:

- Does the app install where users expect?
- Does it appear in the Ubuntu app launcher?
- Are release assets named clearly?
- Can a fresh machine install and run it?
- Does the documentation explain the build and release path?

Those questions are not glamorous, but they are very real.

## Save As

The current feature work is Save As. The important design decision is that a copied project
should be saved into its own folder:

    SelectedParent/
      ProjectName/
        ProjectName.configured

It should not copy the original .git folder by default. A Save As copy should behave like a
standalone project unless the user explicitly initializes or connects Git later. That avoids
accidentally pushing from a copied project to the original repository remote.

## Links

- [CONFIGURED on GitHub](https://github.com/reeceholland/configured)
- [CONFIGURED documentation](https://configured.readthedocs.io/)
