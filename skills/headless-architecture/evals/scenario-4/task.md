# Secure Log Viewer

## Problem/Feature Description

The system administrators need a way to view the latest system logs in their dashboard. The logs are stored in a database and contain a mix of information, including user actions, system events, and some potentially sensitive technical details like internal API keys.

We need to implement a feature that fetches and displays the **last 5 logs** for the admin. 

The security requirements are paramount here. We must ensure that the UI never has direct access to the database or any sensitive information. The database connection and the logic for fetching and sanitizing the logs must be kept in the core application logic. The UI should only receive a "safe" version of the log data that is ready for display.

Please implement the backend logic to fetch and sanitize the logs, the bridge for data transport, and a placeholder for the log viewer UI.

## Output Specification

The output should include:
1.  A core module that fetches logs from the database and sanitizes them (e.g., removing any internal API keys).
2.  The interface or bridge that exposes the sanitized logs to the UI.
3.  A placeholder or example UI component that displays the logs.
4.  Organize your files into a logical directory structure that separates the business logic from the presentation.
