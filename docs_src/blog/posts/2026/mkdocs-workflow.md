---
draft: false
slug: mkdocs-workflow
date: 2026-01-26
categories:
  - mkdocs
---

# Tutorial: A Reproducible MkDocs Workflow with Centralized Templates

Creating documentation or course notes shouldn't involve copy-pasting configuration files and CSS assets for every new project.

In this post, I will share my complete workflow. It uses **MkDocs**, a custom Bash script (`zk`), and a centralized template directory. This allows me to write in **Zettlr** or **Kate** (pure text mode) and instantly preview the result without cluttering my project folders.

<!-- more -->

## Prerequisites

To reproduce this setup, you will need:

* **MkDocs** installed (preferably in a Conda environment or venv).
* **MkDocs Material** theme.
* A Linux environment (Bash).

## Step 1: Directory Structure

First, organize your file system to separate the **Template** (global assets) from your **Projects** (content).

Create a base directory (e.g., `template`).

### The Template Directory

This folder holds the files that are common to all projects: CSS, JavaScript (like KaTeX for math), and fonts .

```text
/path/to/your/template
├── assets
│   ├── extra.css
│   ├── katex/        # Offline math rendering 
│   └── katex.js
├── index.md          # Default homepage if none exists 
└── mkdocs.yml        # Base configuration 

```

### The Base Configuration (`mkdocs.yml`)

Save this as `/path/to/your/template/mkdocs.yml`. It defines the theme, offline settings, and plugins .

```yaml
site_name: Notes
use_directory_urls: false
theme:
  name: material

plugins:
  search: {}
  offline: {}
  autorefs: {}

extra_css:
  - assets/extra.css
  - assets/katex/katex.min.css

extra_javascript:
  - assets/katex.js
  - assets/katex/katex.min.js
  - assets/katex/contrib/auto-render.min.js

markdown_extensions:
  admonition: {}
  pymdownx.details: {}
  pymdownx.arithmatex:
    generic: true
  pymdownx.superfences:
    custom_fences:
      - name: mermaid
        class: mermaid
        format: !!python/name:pymdownx.superfences.fence_code_format

```

## Step 2: The Project Directory

Now, create a project folder (e.g., `event`). Notice how clean it is—it only contains content and a small config file .

```text
/path/to/your/event/
├── index.md
├── my-article.md
└── mkdocs-inherit.yml  # The "Delta" config

```

### The Inheritance Config (`mkdocs-inherit.yml`)

Instead of a full config, this file simply inherits from the base and overrides specific fields (like the site name or color) .

```yaml
INHERIT: mkdocs.yml
site_name: Tech Week 2026.1
theme:
    palette:
        primary: teal
        accent:  teal
nav:
  - Home: index.md
  - About: my-article.md

```

## Step 3: The `zk` Script (The Engine)

This script automates the build process. It creates a temporary directory, links your project files to the template assets, and runs the server .

Save this file as `zk` in your path (e.g., `/usr/local/bin/zk`) and make it executable (`chmod +x zk`).

**Note:** You **must** edit the `BASE` variable to match your template path .

```bash
#!/bin/bash
# Enable extended globbing (pattern matching) to handle numeric arguments later.
shopt -s extglob

#
# USER CONFIGURATION
#

# Default port for the local server
PORT=8000

# Default mode is "serve" (preview). Set to true to generate static HTML files.
BUILD=false

# IMPORTANT: Absolute path to your centralized template folder.
# This folder must contain the 'mkdocs.yml' and the 'assets' folder.
BASE_TEMPLATE="/path/to/your/template" 

# Optional: Activate your Python/Conda environment containing MkDocs.
# Uncomment the line below if you run this outside of an active environment.
# source ~/anaconda3/bin/activate mkdocs

#
# ARGUMENT PARSING
#
# This section checks if the user provided arguments like "b" or a port number.

case $1 in
    "b")
        # Usage: ./zk b
        # Enables build mode (generates the 'site' folder).
        BUILD=true
    ;;
    +([0-9])) 
        # Usage: ./zk 8085
        # If the argument is just a number, treat it as the Port.
        PORT=$1
    ;;
esac

#
# VARIABLES & PREPARATION
#

# Flags to track which temporary links we created (to delete them later)
USING_TEMPLATE=false
LINKED_ASSETS=false
LINKED_INDEX=false
CFG=""

# Save the current directory (Project Folder)
SRC=$PWD

# Define a temporary location for the build process.
# We use /tmp to ensure we don't clutter your home directory.
# ${PWD##*/} extracts just the current folder name (e.g., "event").
TMP="/tmp/mkdocs-build-${PWD##*/}" 

#
# LOGIC: VIRTUAL ENVIRONMENT SETUP
#

# Check: Does this folder already have a full mkdocs.yml?
# If NOT, we assume it's a "Content-Only" project and needs the Template.
if [ ! -f "mkdocs.yml" ]; then
    USING_TEMPLATE=true
    
    # Create a clean temporary directory and move into it
    mkdir -p $TMP
    cd $TMP
    
    # Link the Project Content
    # We create a link named 'docs' pointing to your actual project files.
    # MkDocs expects content to be in a folder named 'docs'.
    ln -sf $SRC docs
    
    # Link the Template Cache (Optional)
    # This helps speed up builds if you have plugins that cache data.
    ln -sf $BASE_TEMPLATE/.cache .

    # Link the Base Config
    ln -sf $BASE_TEMPLATE/mkdocs.yml .

    # Inject Assets (CSS/JS)
    # If your project doesn't have an 'assets' folder, we borrow the template's.
    # We link it inside your source folder so the preview works correctly.
    if [ ! -d "$SRC/assets" ]; then
        LINKED_ASSETS=true
        ln -s $BASE_TEMPLATE/assets $SRC/
    fi

    # Inject Default Homepage
    # If you haven't written an index.md yet, use the generic one from the template.
    if [ ! -f "$SRC/index.md" ]; then
        LINKED_INDEX=true
        ln -s $BASE_TEMPLATE/index.md $SRC/
    fi

    # Handle Configuration Inheritance
    # If the project has a 'mkdocs-inherit.yml' (Delta config), we use it.
    if [ -f "$SRC/mkdocs-inherit.yml" ]; then
        # Tell MkDocs to use this specific file
        CFG="-f mkdocs-inherit.yml"       
        # Link the inherit file to the temp dir
        ln -sf $SRC/mkdocs-inherit.yml .
    fi
fi

#
# EXECUTION
#

if $BUILD; then
    # Generates the static site (usually into a 'site' folder)
    echo "Building static site..."
    mkdocs build $CFG
else
    # Starts the live preview server.
    # --dirty: Only rebuilds modified files (faster).
    # --clean: Cleans up stale artifacts before starting.
    # -a: Binds to 0.0.0.0 to allow access from other devices on the network.
    echo "Starting local server on port $PORT..."
    mkdocs serve --livereload --dirty --clean -a 0.0.0.0:$PORT $CFG
fi

#
# CLEANUP
#
# This runs after you stop the server (Ctrl+C).

if $USING_TEMPLATE; then
    # Go back to the original folder
    cd $SRC
    
    # Delete the temporary build environment
    rm -rf $TMP
fi

# Remove the temporary 'assets' link from your project folder
if $LINKED_ASSETS; then
    rm "$SRC/assets"
fi

# Remove the temporary 'index.md' link from your project folder
if $LINKED_INDEX; then
    rm "$SRC/index.md"
fi

# End of script

```

## Step 4: Daily Usage

With the setup complete, here is the workflow:

1. **Open your project** folder.
2. **Edit content** using your preferred tool:
    * **Zettlr:** Great for a "notebook" feel with Markdown highlighting.
    * **Kate:** Excellent for a lightweight, **pure text** editing experience.
3. **Run the script:**
Open a terminal in the folder and type:
```bash
zk
```
Or specifying a port:
```bash
zk 8085
```
4. It's possible to edit the Markdown file; MkDocs' live reload will refresh the page in your browser.



The script will handle the linking, launch the server, and clean up the temporary links when you press `Ctrl+C`. This keeps your source directory perfectly clean, containing only your text files.
