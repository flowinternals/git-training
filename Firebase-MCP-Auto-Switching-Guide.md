# Firebase MCP Auto-Switching with ADC Setup Guide

This comprehensive guide covers setting up Firebase MCP auto-switching between projects using Application Default Credentials (ADC) for secure authentication.

## What This Solves

- **Problem**: Firebase MCP tool was detecting wrong project when switching between workspaces
- **Solution**: Project-specific MCP configurations with ADC authentication
- **Result**: Firebase MCP tool automatically connects to the correct project with secure authentication

## Overview

This setup provides:
- **Automatic project switching** based on Cursor workspace
- **Secure authentication** using Application Default Credentials (ADC)
- **Consistent configuration** across multiple Firebase projects
- **No service account key management** required

## Prerequisites

- Windows environment
- Node.js and npm installed
- Firebase CLI installed and authenticated
- Admin access to Google Cloud Console

## Step 1: Install Google Cloud SDK

**Download and Install:**
```cmd
powershell -Command "& {Invoke-WebRequest -Uri 'https://dl.google.com/dl/cloudsdk/channels/rapid/GoogleCloudSDKInstaller.exe' -OutFile 'GoogleCloudSDKInstaller.exe'}"
GoogleCloudSDKInstaller.exe
```

**During Installation:**
- Check "Add gcloud to PATH"
- Choose "Install for all users" if you have admin rights

## Step 2: Set Up Application Default Credentials (ADC)

### Authenticate with Google Cloud
```cmd
gcloud auth application-default login
```

**Sign in with the same account used for Firebase CLI** (e.g., `your-project@gmail.com`)

### Configure Project Settings

**For ITC2 Website:**
```cmd
gcloud config set project itc2-website
gcloud auth application-default set-quota-project itc2-website
```

**For FlowInternals Website:**
```cmd
gcloud config set project flowinternals-website
gcloud auth application-default set-quota-project flowinternals-website
```

## Step 3: Create Project-Specific MCP Configurations

### ITC2 Website Project (`D:\Active\itc2-website`)

**Create `.cursor/mcp.json`:**
```json
{
  "mcpServers": {
    "firebase-itc2": {
      "command": "npx",
      "args": [
        "-y",
        "firebase-tools@latest",
        "mcp"
      ],
      "env": {
        "FIREBASE_PROJECT_ID": "itc2-website"
      }
    }
  }
}
```

**Verify `.firebaserc` exists:**
```json
{
  "projects": {
    "default": "itc2-website"
  },
  "targets": {},
  "etags": {}
}
```

### FlowInternals Website Project (`D:\Active\flowinternals-website`)

**Create `.cursor/mcp.json`:**
```json
{
  "mcpServers": {
    "firebase-flowinternals": {
      "command": "npx",
      "args": [
        "-y",
        "firebase-tools@latest",
        "mcp"
      ],
      "env": {
        "FIREBASE_PROJECT_ID": "flowinternals-website"
      }
    }
  }
}
```

**Verify `.firebaserc` exists:**
```json
{
  "projects": {
    "default": "flowinternals-website"
  }
}
```

## Step 4: Verify Setup

### Check Google Cloud Configuration
```cmd
gcloud --version
gcloud config get-value project
gcloud auth list
```

### Check Firebase CLI
```cmd
firebase use
```

### Test ADC Authentication
```cmd
gcloud auth application-default print-access-token
```

### Test MCP Server
```cmd
npx firebase-tools@latest mcp
```

Expected output:
```
This is a running process of the Firebase MCP server. This command should only be executed by an MCP client...
```

## How It Works

1. **Cursor loads `.cursor/mcp.json`** when a project opens
2. **MCP server starts** with project-specific environment variables
3. **ADC authentication** provides secure credentials automatically
4. **Project detection** uses `FIREBASE_PROJECT_ID` environment variable
5. **Firebase MCP tool** connects to the correct project

## Usage

### Automatic Switching
1. **Open ITC2 Website** in Cursor → `firebase-itc2` MCP server connects to `itc2-website`
2. **Open FlowInternals Website** in Cursor → `firebase-flowinternals` MCP server connects to `flowinternals-website`
3. **Switch between projects** → Firebase MCP automatically switches

### Manual Project Switching
If you need to switch Google Cloud project manually:
```cmd
# Switch to ITC2 project
gcloud config set project itc2-website
gcloud auth application-default set-quota-project itc2-website

# Switch to FlowInternals project
gcloud config set project flowinternals-website
gcloud auth application-default set-quota-project flowinternals-website
```

## Testing

### Test Project Detection
1. Open ITC2 Website in Cursor
2. Check MCP servers - `firebase-itc2` should be active
3. Verify Firebase MCP tool shows `itc2-website` project
4. Open FlowInternals Website in Cursor
5. Check MCP servers - `firebase-flowinternals` should be active
6. Verify Firebase MCP tool shows `flowinternals-website` project

### Test ADC Authentication
```cmd
# Test ADC is working
gcloud auth application-default print-access-token

# Test Firebase CLI
firebase use

# Test MCP server startup
npx firebase-tools@latest mcp
```

## Troubleshooting

### Firebase MCP Tool Shows Wrong Project
1. **Restart Cursor** - MCP configuration is loaded on startup
2. **Check `.cursor/mcp.json`** - Ensure it exists and is valid JSON
3. **Check environment variables** - Verify `FIREBASE_PROJECT_ID` is set correctly
4. **Check `.firebaserc`** - Ensure it has the correct project ID

### ADC Authentication Issues
1. **Re-authenticate** - Run `gcloud auth application-default login`
2. **Check project** - Run `gcloud config get-value project`
3. **Set quota project** - Run `gcloud auth application-default set-quota-project YOUR_PROJECT_ID`
4. **Verify credentials** - Run `gcloud auth application-default print-access-token`

### MCP Server Errors
1. **Check Node.js** - Ensure Node.js is installed
2. **Check Firebase CLI** - Ensure Firebase CLI is installed and authenticated
3. **Check project access** - Verify you have access to the Firebase project
4. **Check ADC** - Ensure ADC is properly configured

### Project Resolution Issues
1. **Check `.firebaserc`** - Ensure it has `projects.default` set
2. **Check environment variables** - Clear any conflicting `FIREBASE_PROJECT_ID`
3. **Check gcloud** - Run `gcloud config get-value project` to see active project

## Benefits of ADC

- **No service account keys** to manage
- **Automatic credential refresh** when you re-authenticate
- **Secure** - credentials stored in Google Cloud SDK's secure storage
- **Simple** - no additional setup required
- **Consistent** - works across all Google Cloud services

## Security Notes

- ADC credentials are stored locally in `%APPDATA%\gcloud\application_default_credentials.json`
- These credentials are automatically refreshed
- No need to manage or rotate service account keys
- More secure than downloading JSON key files

## Cost Information

- **Firebase MCP server**: FREE
- **ADC authentication**: FREE
- **Firebase services**: You only pay for Firebase services you actually use
- **Free tiers**: Most Firebase services have generous free tiers

## Benefits

✅ **Automatic project switching** - No manual configuration needed  
✅ **Workspace-based detection** - Correct project based on open workspace  
✅ **Secure authentication** - ADC instead of service account keys  
✅ **Consistent setup** - Same configuration across all projects  
✅ **Cross-platform** - Works on Windows, macOS, and Linux  
✅ **Future-proof** - Easy to add new Firebase projects  

## Portable Setup for Other Projects

### Making the Setup Portable

This guide can be used for **any Firebase project** with **any Google account**. Here's how to adapt it:

#### 1. Portable Batch Script

Create a `firebase-switch.bat` file in your project's `scripts` folder:

```batch
@echo off
REM Portable Firebase Project Switcher
REM Automatically detects Firebase project from .firebaserc file

echo ========================================
echo Portable Firebase Project Switcher
echo ========================================

REM Get current directory
set CURRENT_DIR=%CD%
echo Current directory: %CURRENT_DIR%

REM Check if .firebaserc exists
if not exist ".firebaserc" (
    echo ❌ .firebaserc not found in current directory
    echo This directory is not a Firebase project
    echo.
    echo Please run this script from a Firebase project directory
    echo that contains a .firebaserc file
    pause
    exit /b 1
)

REM Extract project ID from .firebaserc
for /f "tokens=2 delims=:" %%a in ('findstr "default" .firebaserc') do (
    set PROJECT_ID=%%a
)

REM Clean up the project ID (remove quotes, commas, spaces)
set PROJECT_ID=%PROJECT_ID:"=%
set PROJECT_ID=%PROJECT_ID:,=%
set PROJECT_ID=%PROJECT_ID: =%

if "%PROJECT_ID%"=="" (
    echo ❌ Could not determine Firebase project from .firebaserc
    echo Please ensure .firebaserc contains a "default" project entry
    pause
    exit /b 1
)

echo Detected Firebase project: %PROJECT_ID%
echo.

REM Switch to the detected project
echo Switching to %PROJECT_ID% project...
firebase use %PROJECT_ID%
if %errorlevel% equ 0 (
    echo ✅ Switched to %PROJECT_ID%
) else (
    echo ❌ Failed to switch to %PROJECT_ID%
    echo.
    echo Possible issues:
    echo - Project %PROJECT_ID% does not exist
    echo - You don't have access to this project
    echo - Firebase CLI is not authenticated
    echo.
    echo Try running: firebase login
    pause
    exit /b 1
)

echo.
echo Current Firebase project:
firebase use
echo.
echo ========================================
echo Firebase project switch completed!
echo ========================================
pause
```

#### 2. Portable MCP Configuration

For any new project, create `.cursor/mcp.json` with this template:

```json
{
  "mcpServers": {
    "firebase-[PROJECT-NAME]": {
      "command": "npx",
      "args": [
        "-y",
        "firebase-tools@latest",
        "mcp"
      ],
      "env": {
        "FIREBASE_PROJECT_ID": "[YOUR-PROJECT-ID]"
      }
    }
  }
}
```

**Replace:**
- `[PROJECT-NAME]` with a unique identifier (e.g., `firebase-myapp`)
- `[YOUR-PROJECT-ID]` with your actual Firebase project ID

#### 3. Portable Setup Steps

For **any new Firebase project**:

1. **Copy this guide** to the new project's `scripts` folder
2. **Create `.firebaserc`** with your project ID:
   ```json
   {
     "projects": {
       "default": "your-project-id"
     }
   }
   ```
3. **Create `.cursor/mcp.json`** using the template above
4. **Run ADC setup** (Steps 1-4 from this guide) for the new Google account
5. **Test** by running the portable batch script

### Account-Specific Notes

- **Different Google accounts**: Each account needs its own ADC setup
- **Different Firebase projects**: Each project needs its own `.firebaserc` and `.cursor/mcp.json`
- **Team projects**: ADC works with any Google account that has access to the Firebase project

## Next Steps

1. **Test the setup** by opening both projects in Cursor
2. **Verify Firebase MCP tool** shows the correct project
3. **Monitor usage** in Firebase Console → Project Settings → Usage
4. **Add new projects** by creating similar `.cursor/mcp.json` configurations

## File Structure

### ITC2 Website Project
```
D:\Active\itc2-website\
├── .cursor/
│   └── mcp.json                    # MCP configuration
├── .firebaserc                     # Firebase project config
└── firebase.json                   # Firebase services config
```

### FlowInternals Website Project
```
D:\Active\flowinternals-website\
├── .cursor/
│   └── mcp.json                    # MCP configuration
├── .firebaserc                     # Firebase project config
└── firebase.json                   # Firebase services config
```

---

**Created for:** ITC2 Website and FlowInternals Website Projects  
**Date:** October 10, 2025  
**Contact:** flowinternals@gmail.com  
**Status:** ✅ Complete and Tested