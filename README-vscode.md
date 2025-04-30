# Vscode Tips and Tricks

## Key Sequence Shortcuts

- **Command Palette**: `Cmd + Shift + P` (Mac) - Access all available commands.
- **Quick Open**: `Cmd + P` (Mac) - Quickly open files.
- **Toggle Terminal**: `Ctrl + \`` (Mac) - Open/close the integrated terminal.
- **Find in Files**: `Cmd + Shift + F` (Mac) - Search across files.
- **Multi-line Edit**: `Option + Click` (Mac) - Place multiple cursors for editing.

### Detailed Explanation

1.  **Placing Multiple Cursors**:
    *   By holding down the `Option` key and clicking at different points in your text, you can place multiple cursors in the editor. Each click adds a new cursor at the clicked location.

2.  **Simultaneous Editing**:
    *   Once multiple cursors are placed, any text you type will appear at each cursor's location. This means you can insert, delete, or modify text in several places at once.

3.  **Selecting Text**:
    *   You can also select text with multiple cursors. Hold `Option` and drag to select text across multiple lines. This is useful for replacing or modifying text blocks.

4.  **Using Keyboard Shortcuts**:
    *   You can use keyboard shortcuts like `Cmd + D` to select the next occurrence of the current selection, adding a new cursor at each occurrence. This is particularly useful for renaming variables or changing repeated text.

## Tips and Tricks

-   **Zen Mode**: `Cmd + K Z` (Mac) - Focus on coding by hiding all UI elements.
-   **Split Editor**: `Cmd + \` (Mac) - Split the editor to view multiple files side by side.
-   **Auto Save**: Enable auto-save in settings to prevent data loss.

### How to Enable Auto Save

1.  Open the Command Palette using `Cmd + Shift + P`.
2.  Type "Preferences: Open Settings" and press Enter.
3.  In the search bar, type "auto save".
4.  Set "Files: Auto Save" to "afterDelay" or "onWindowChange" based on your preference.
5.  Adjust the "Files: Auto Save Delay" if needed to control the delay time before saving.

# VS Code Settings

**Note:** This document details a set of Visual Studio Code settings, i.e. the `settings.json` file.

## VS Code `settings.json` Configuration

This configuration represents a combination of settings from `iterm2-air-mark`, `iterm2-air-work`, `iterm2-pro-mark`, and `iterm2-pro-work` snippets. Conflicting values were resolved by choosing the most frequent value or a sensible default.

```json
{
    "files.exclude": {
        "**/.mp4": true
    },
    "vim.disableExtension": true,
    "git.confirmSync": false,
    "git.postCommitCommand": "sync",
    "git.autofetch": true,
    "git.pruneOnFetch": true,
    "github.copilot.enable": {
        "scminput": true
    },
    "terminal.integrated.enableMultiLinePasteWarning": "never",
    "terminal.integrated.fontWeightBold": "bold",
    "terminal.integrated.altClickMovesCursor": false,
    "terminal.integrated.enablePersistentSessions": false,
    "terminal.integrated.shellIntegration.decorationsEnabled": "never",
    "terminal.integrated.shellIntegration.enabled": false,
    "terminal.integrated.scrollback": 5000,
    "terminal.integrated.defaultProfile.osx": "bash",
    "workbench.colorTheme": "Tomorrow Night Blue",
    "python.terminal.activateEnvironment": false,
    "intersystems.language-server.suggestTheme": false,
    "intersystems.language-server.trace.server": "verbose",
    "window.autoDetectHighContrast": false,
    "window.zoomLevel": 1,
    "editor.minimap.enabled": false,
    "editor.cursorWidth": 3,
    "editor.autoIndent": "brackets",
    "editor.fontSize": 14,
    "yaml.schemas": {
        "file:///Users/marmorri/.vscode/extensions/atlassian.atlascode-3.6.0/resources/schemas/pipelines-schema.json": "bitbucket-pipelines.yml"
    },
    "atlascode.jira.enabled": false,
    "atlascode.bitbucket.enabled": true,
    "redhat.telemetry.enabled": false,
    "[python]": {
        "editor.formatOnType": true
    }
}
```

## Explanation of Settings

Here's a breakdown of each setting in the merged configuration:

*   `"files.readonlyExclude": { "*.py": true }`
    *   **What it does:** Excludes files matching these glob patterns (`*.py`) from being marked as read-only by the `files.readonlyFromPermissions` setting. Even if their file system permissions are read-only, VS Code will treat them as editable.
    *   **Interaction:** Works in conjunction with `files.readonlyFromPermissions`.

*   `"files.readonlyFromPermissions": true`
    *   **What it does:** Makes files that have read-only permissions on the file system appear as read-only (locked) in the VS Code editor.
    *   **Interaction:** Affected by `files.readonlyExclude`.

*   `"vim.disableExtension": true`
    *   **What it does:** Disables the VSCodeVim extension if it's installed. Set to `false` or remove the line to enable it.

*   `"git.enableSmartCommit": true`
    *   **What it does:** Allows staging all changes automatically when committing if there are no currently staged changes. This simplifies the commit process slightly.

*   `"git.confirmSync": false`
    *   **What it does:** Disables the confirmation prompt that normally appears before running the `Git: Sync` command (which typically performs a pull and then a push).
    *   **Interaction:** Works with the `Git: Sync` command or features that trigger it (like `git.postCommitCommand`).

*   `"git.postCommitCommand": "sync"`
    *   **What it does:** Automatically runs the `Git: Sync` command after a successful commit.
    *   **Interaction:** Relies on `git.confirmSync: false` for a smoother, non-interactive sync after commit.

*   `"github.copilot.enable": { "scminput": true }`
    *   **What it does:** Specifically enables GitHub Copilot suggestions within the Source Control commit message input box. Other Copilot features might be controlled elsewhere or by default.

*   `"terminal.integrated.enableMultiLinePasteWarning": "never"`
    *   **What it does:** Disables the warning prompt that usually appears when pasting text containing multiple lines into the integrated terminal. Use with caution, as this can prevent accidental execution of harmful multi-line scripts.

*   `"terminal.integrated.fontWeightBold": "bold"`
    *   **What it does:** Sets the font weight for bold text within the integrated terminal.

*   `"terminal.integrated.altClickMovesCursor": false`
    *   **What it does:** Prevents Alt+Click (or Option+Click on macOS) from moving the cursor in the integrated terminal. When `true`, it allows positioning the cursor similar to a text editor, which can interfere with some terminal applications.

*   `"terminal.integrated.enablePersistentSessions": false`
    *   **What it does:** Disables the experimental feature where terminal sessions and their history are restored after restarting VS Code. When `false`, terminals start fresh.

*   `"workbench.settings.applyToAllProfiles": []`
    *   **What it does:** Specifies a list of settings that should *not* be applied to all profiles when using VS Code Profiles. An empty list `[]` means all settings *can* be profile-specific unless explicitly shared.

*   `"workbench.colorTheme": "Tomorrow Night Blue"`
    *   **What it does:** Sets the overall color theme for the VS Code workbench UI.

*   `"python.terminal.activateEnvironment": false`
    *   **What it does:** Prevents the Python extension from automatically attempting to activate a selected Python environment when a new terminal is opened.

*   `"terminal.integrated.shellIntegration.decorationsEnabled": "never"`
    *   **What it does:** Disables the visual decorations (like the colored dots/marks in the gutter) added by the terminal's shell integration feature.

*   `"terminal.integrated.shellIntegration.enabled": false`
    *   **What it does:** Completely disables the enhanced shell integration features in the integrated terminal (command tracking, current working directory detection, etc.). Disabling this might slightly improve performance but removes features like command navigation.

*   `"terminal.integrated.shellIntegration.showCommandGuide": false`
    *   **What it does:** Hides the command guide decorations (lines showing command start/end) provided by shell integration. Only relevant if `terminal.integrated.shellIntegration.enabled` is `true`.

*   `"intersystems.language-server.suggestTheme": false`
    *   **What it does:** Prevents the InterSystems Language Server extension from suggesting a specific color theme.

*   `"intersystems.language-server.trace.server": "verbose"`
    *   **What it does:** Sets the logging level for the InterSystems Language Server to verbose, providing detailed diagnostic information in the Output panel.

*   `"window.autoDetectHighContrast": false`
    *   **What it does:** Prevents VS Code from automatically switching to a high contrast theme if it detects the OS is using one.

*   `"editor.minimap.enabled": false`
    *   **What it does:** Hides the minimap (the code overview scrollbar on the right side of the editor).

*   `"yaml.schemas": { ... }`
    *   **What it does:** Associates a specific JSON schema (for Bitbucket Pipelines, provided by the AtlasCode extension) with files named `bitbucket-pipelines.yml`. This enables validation and autocompletion for that file type based on the schema.

*   `"atlascode.jira.enabled": false`
    *   **What it does:** Disables the Jira integration features within the Atlassian Atlascode extension.

*   `"atlascode.bitbucket.enabled": true`
    *   **What it does:** Enables the Bitbucket integration features within the Atlassian Atlascode extension.

*   `"redhat.telemetry.enabled": false`
    *   **What it does:** Disables telemetry data collection for Red Hat extensions (like the YAML extension).

*   `"terminal.integrated.scrollback": 5000`
    *   **What it does:** Sets the maximum number of lines kept in the integrated terminal's scrollback buffer for the current session to 5000.

*   `"terminal.integrated.persistentSessionScrollback": 5000`
    *   **What it does:** Sets the maximum number of lines kept in the scrollback buffer for persistent terminal sessions (if enabled). Only relevant if `terminal.integrated.enablePersistentSessions` is `true`.

*   `"window.zoomLevel": 1`
    *   **What it does:** Adjusts the zoom level for the entire VS Code window UI. `0` is default, positive numbers zoom in, negative numbers zoom out. `1` means slightly zoomed in.

*   `"[python]": { "editor.formatOnType": true }`
    *   **What it does:** Enables automatic code formatting *as you type* specifically for Python files. This relies on having a Python formatter configured (like Black or Autopep8).

*   `"editor.fontSize": 14`
    *   **What it does:** Sets the font size for the text editor.

*   `"files.exclude": { "**/.mp4": true }`
    *   **What it does:** Hides files matching this pattern (`.mp4` files anywhere in the workspace) from the VS Code Explorer file tree.

*   `"git.openDiffOnClick": false`
    *   **What it does:** Prevents VS Code from automatically opening the diff view when clicking on a file in the Source Control view. You would need to explicitly click an "Open Changes" icon or similar.

*   `"editor.cursorWidth": 3`
    *   **What it does:** Sets the width of the editor cursor in pixels.

*   `"editor.autoIndent": "brackets"`
    *   **What it does:** Configures automatic indentation. `"brackets"` means the editor will indent based on bracket pairs (`{}`, `[]`, `()`). Other options include `"full"` (more language-aware) or `"none"`.

*   `"terminal.integrated.defaultProfile.linux": ""`
    *   **What it does:** Specifies the default terminal profile to use on Linux. An empty string likely means VS Code will try to auto-detect or use its default.

*   `"terminal.integrated.defaultProfile.osx": "bash"`
    *   **What it does:** Explicitly sets the default terminal profile to use the standard `bash` shell on macOS, overriding any potential auto-detection (like zsh).





