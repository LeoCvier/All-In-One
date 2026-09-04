# All-In-One Tool

The **All-In-One Tool** is a desktop application that combines useful tools and system functions into one clean, modular interface.

The goal is to make the application feel like a small personal desktop environment/control center rather than a collection of unrelated programs.

## Project Status

🚧 **Work in Progress**

The project is currently being developed on **Windows**.

The long-term goal is to support multiple desktop platforms, with Linux support remaining an important target.

---

## Technology Stack

### Frontend

- React
- TypeScript / JavaScript
- HTML
- CSS

### Desktop

- Tauri
- Rust

### Data

- SQLite

The general architecture is:

```text
React
  ↓
Tauri
  ↓
Rust
  ↓
Operating System
```

---

# Main Concept

The application is designed around four main areas:

```text
┌──────────────────────────────────────────────────────┐
│ All-In-One     CPU: xx%   RAM: xx%     [tools] ☆ User│
├────┬─────────────────────────────────────────────────┤
│    │                                                 │
│    │              APPLICATION AREA                   │
│ 🧮 │                                                 │
│ 📁 │                                                 │
│ 🌐 │                                                 │
│ 💬 │                                                 │
│ ⚙️ │                                                 │
│    │                                                 │
└────┴─────────────────────────────────────────────────┘
```

The exact design is still evolving.

---

# Sidebar

The sidebar contains the available internal applications.

Planned applications include:

- Calculator
- Files
- Browser
- Chat / Social
- System
- Settings
- Other future applications

The sidebar should remain compact by default and expand when the user moves the mouse over it.

Collapsed:

```text
│ 🧮 │
│ 📁 │
│ 🌐 │
│ 💬 │
│ ⚙️ │
```

Expanded:

```text
│ 🧮 Calculator │
│ 📁 Files      │
│ 🌐 Browser    │
│ 💬 Chat       │
│ ⚙️ Settings   │
```

---

# Internal Application System

The internal tools are designed as separate modules rather than one large component.

Possible structure:

```text
apps/
├── calculator/
├── files/
├── browser/
├── chat/
├── system/
├── settings/
└── ...
```

The application manager controls which applications are available, open, and displayed.

This should make it possible to add new applications without rewriting the entire application.

---

# Screen Split

One of the main features is the ability to display up to **four internal applications simultaneously**.

### One application

```text
┌───────────────────────┐
│                       │
│       Browser         │
│                       │
└───────────────────────┘
```

### Two applications

```text
┌──────────────┬────────┐
│              │        │
│    Files     │  Chat  │
│              │        │
└──────────────┴────────┘
```

### Three applications

```text
┌──────────────┬────────┐
│              │Browser │
│    Files     ├────────┤
│              │  Chat  │
└──────────────┴────────┘
```

### Four applications

```text
┌──────────────┬──────────────┐
│   Browser    │     Chat     │
├──────────────┼──────────────┤
│    Files     │ Calculator   │
└──────────────┴──────────────┘
```

The layout should be dynamic.

When applications are opened or closed, or when the window is resized, the application areas should automatically adapt.

---

# Resource Monitor

The top bar should provide a quick overview of system resources.

Example:

```text
CPU: 23%
RAM: 47%
```

Possible future information:

- CPU usage
- RAM usage
- GPU usage
- GPU memory
- Disk usage
- Network activity
- Temperatures

System information should preferably be provided through the Tauri/Rust backend rather than relying on arbitrary shell commands from the frontend.

---

# Calculator

The calculator will be an internal application.

Possible functionality:

- Basic arithmetic
- Scientific functions
- Calculation history
- Copy result
- Keyboard support

---

# File Application

The project is planned to contain an internal file-management application.

Possible functionality:

- Browse directories
- Open files
- Basic file operations
- Search
- Favorites
- Recently opened files
- File information

The exact permission model will be designed during development.

---

# Favorites

The application should contain a star/favorites system.

Example:

```text
⭐ Important Project
⭐ School
⭐ Config
⭐ Important File
```

Favorites should persist between application launches.

SQLite may be used to store this information.

Possible database structure:

```text
favorites
---------
id
name
path
created_at
```

The system could later be expanded to support applications, folders, websites, settings, and other items.

---

# Browser

The project may contain a small internal browser.

Possible functionality:

- Address bar
- Back
- Forward
- Reload
- Tabs
- Start page
- Favorites
- Downloads
- Basic browser settings

Possible structure:

```text
apps/
└── browser/
    ├── Browser
    ├── Tabs
    ├── AddressBar
    └── BrowserSettings
```

The goal is initially to provide a useful mini browser rather than replace full browsers such as Firefox, Chromium, or Brave.

---

# Chat / Social

The application should contain a Chat/Social area.

Possible services include:

- Mail
- WhatsApp
- Discord
- Other communication services

The project should not attempt to recreate every service from scratch.

Depending on the service, integrations may use:

- Web interfaces
- Official APIs
- Other supported integrations

The Chat/Social system should remain modular.

---

# Dynamic Layout

The application should automatically resize its internal areas when additional applications or the chat area are opened.

For example:

```text
Without Chat:

┌─────────────────────────────┐
│                             │
│       Main Application      │
│                             │
└─────────────────────────────┘
```

With Chat:

```text
┌──────────────────────┬──────┐
│                      │ Chat │
│   Main Application   │      │
│                      │      │
└──────────────────────┴──────┘
```

The layout manager should handle this dynamically.

---

# User Menu

The top-right area contains a user menu.

Possible functionality:

- User profile
- Settings
- Login
- Logout
- Security options
- Application preferences
- Access/logging information

The exact authentication system is still being designed.

---

# Security

A future security system should protect sensitive information from other users of the computer.

Possible functionality:

- User accounts
- Login/logout
- Password protection
- Lock screen
- Automatic locking
- Protected sections

Passwords must never be stored in plain text.

An established password-hashing/authentication solution should be used.

---

# Activity & Access Logs

The application is planned to have a logging system.

Possible categories:

```text
System
Security
User activity
Application events
Network
Errors
```

Example:

```text
[INFO] Browser opened
[INFO] User logged in
[WARN] Failed authentication
[INFO] Settings changed
[ERROR] Application failed to start
```

Logs should be transparent and should only collect information that is actually required.

---

# Cloud

Cloud functionality is **not part of the first implementation**.

A future version may contain:

- File synchronization
- Remote access
- Settings synchronization
- Device management
- Account-based storage
- Secure remote connections

The initial versions should focus on building a stable local desktop application.

---

# Architecture

High-level architecture:

```text
                    ALL-IN-ONE TOOL
                           │
             ┌─────────────┴─────────────┐
             │                           │
        React Frontend              Tauri Desktop
        HTML/CSS/TS                      │
             │                           │
             │                         Rust
             │                           │
             └─────────────┬─────────────┘
                           │
                    Windows / Linux
```

Frontend:

```text
React
TypeScript
CSS
```

Desktop layer:

```text
Tauri
Rust
```

Persistence:

```text
SQLite
```

---

# Project Structure

A possible project structure:

```text
all-in-one-tool/
│
├── src/
│   ├── app/
│   │   ├── App.tsx
│   │   ├── AppManager.ts
│   │   └── LayoutManager.ts
│   │
│   ├── components/
│   │   ├── Sidebar/
│   │   ├── Header/
│   │   ├── UserMenu/
│   │   ├── ResourceMonitor/
│   │   ├── AppContainer/
│   │   └── SplitView/
│   │
│   ├── apps/
│   │   ├── calculator/
│   │   ├── files/
│   │   ├── browser/
│   │   ├── chat/
│   │   ├── system/
│   │   ├── settings/
│   │   └── cloud/
│   │
│   ├── services/
│   │   ├── favorites/
│   │   ├── logging/
│   │   ├── authentication/
│   │   └── system/
│   │
│   └── styles/
│
├── src-tauri/
│   ├── src/
│   │   ├── main.rs
│   │   ├── system.rs
│   │   ├── filesystem.rs
│   │   ├── logging.rs
│   │   └── authentication.rs
│   │
│   └── tauri.conf.json
│
└── database/
```

This is a proposed structure and can change as the project develops.

---

# Development Roadmap

The project should be developed incrementally.

## Version 0.1 – Foundation

Build:

- Tauri
- React
- TypeScript
- Main window
- Header
- Sidebar
- Basic styling

Goal:

```text
A real desktop application opens on Windows.
```

Linux compatibility should be considered during development so that platform-specific code does not unnecessarily prevent future Linux builds.

---

## Version 0.2 – Internal Applications

Add:

- App manager
- Calculator
- Settings
- Application switching
- Sidebar hover behavior

---

## Version 0.3 – Screen Split

Add:

- 1 application
- 2 applications
- 3 applications
- 4 applications
- Automatic resizing
- Dynamic layouts

---

## Version 0.4 – System Information

Add:

- CPU
- RAM
- Disk
- Network
- Optional GPU information

---

## Version 0.5 – Files & Favorites

Add:

- File browser
- Favorites
- SQLite
- Recently used items

---

## Version 0.6 – Browser

Add:

- Embedded browser/webview
- URL bar
- Navigation
- Tabs
- Basic browser settings

---

## Version 0.7 – Chat / Social

Add:

- Chat menu
- Service integrations
- Chat panel
- Automatic layout resizing

---

## Version 0.8 – Security

Add:

- User system
- Login/logout
- Password protection
- Lock
- Activity logs
- Access logs

---

## Version 0.9 – Polish

Add:

- Animations
- Keyboard shortcuts
- Better error handling
- Themes
- Performance improvements
- Accessibility

---

## Version 1.0 – Stable Release

The first stable release should focus on the local desktop application.

After the local application is stable, future development can focus on:

- Cloud functionality
- Remote functionality
- More integrations
- Linux packaging
- Additional platform support

---

# Platform Support

The current development environment is **Windows**.

The project is intended to remain portable where practical.

Possible future targets include:

```text
Windows
Linux
```

Platform-specific functionality should be isolated where possible.

For example:

```text
React
   ↓
Tauri
   ↓
Rust
   ↓
Platform-specific functionality
```

Windows-specific and Linux-specific functionality should not unnecessarily leak into the React application.

---

# Important Design Principle

The application should feel like **one coherent desktop environment**, not a collection of random mini-programs.

The user should be able to:

```text
Open All-In-One
       ↓
Choose an application
       ↓
Use several applications simultaneously
       ↓
Quickly access files, chats and system information
       ↓
Manage everything from one interface
```

The interface should remain simple even as the underlying functionality becomes more powerful.

---

# Long-Term Vision

The finished application should become a customizable **desktop productivity and control center**.

Example:

```text
┌────────────────────────────────────────────────────┐
│ All-In-One   CPU 23%   RAM 47%       ☆   User      │
├──────┬─────────────────────────────────────────────┤
│      │                                             │
│ 🧮   │  Browser              │ Chat                │
│ 📁   │                       │                     │
│ 🌐   │                       │                     │
│ 💬   ├───────────────────────┼─────────────────────┤
│ ⚙️   │  Files                │ Calculator          │
│      │                       │                     │
└──────┴─────────────────────────────────────────────┘
```

Everything should remain modular.

Users should be able to choose which applications are open, how they are arranged, what is favorited, and which features are available.

The long-term goal is to combine tools, system information, files, communication, browser functionality, security, logging, and eventually cloud functionality into one coherent desktop application.

---

# Current Knowledge & Learning Goals

The project is also intended as a learning project.

Current knowledge:

- HTML
- CSS
- JavaScript
- React
- Linux/Kubuntu

Technologies to learn more deeply:

- Tauri
- Rust
- SQLite
- Desktop application architecture

The project should therefore be developed step-by-step rather than trying to implement every planned feature immediately.

The immediate goal is:

**Create a minimal Tauri + React + TypeScript desktop application on Windows, then recreate the main UI before implementing advanced functionality.**

---

## Project Repository

The project is hosted on GitHub:

`https://github.com/LeoCvier/All-In-One`

The repository may change over time, so the README and repository should be treated as the current source of truth for the project's implementation status.
