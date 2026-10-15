# RunningHub for Affinity — Automatic Installation (Windows)

**Recommended installation method**

This guide explains how to install and configure RunningHub for Affinity using the Windows installer.

You do not need Python to use this installation method.

## Step 1 — Download and install

1. Download `RunningHub-for-Affinity-v1.0.0-Setup.exe` from the project's GitHub Releases page.
2. Double-click the downloaded file.
3. Follow the installation instructions until the process is complete.

The installer will automatically create the necessary files and folders in the following location:

`%LOCALAPPDATA%\RunningHubForAffinity\`

Inside this location, you will find three main folders:

- **Bridge:** Contains the files and scripts required for communication between Affinity and RunningHub.
- **Exchange:** Stores temporary images and results exchanged between Affinity and RunningHub.
- **Preferences:** Stores configuration and preferences used by the integration.

You do not need to move or modify these folders.

## Step 2 — Verify that the connector is running

RunningHub for Affinity uses a small background program called the **connector**.

The connector handles communication between Affinity and RunningHub. Its main process is based on `conector.py`, which is included in the installed application.

The connector starts automatically after installation and uses local port **8765** to communicate with Affinity.

You can verify that it is running:

1. Open Windows PowerShell.
2. Copy and run the following command:

```powershell
Get-NetTCPConnection -LocalPort 8765 -State Listen -ErrorAction SilentlyContinue
```

If the connector is running, PowerShell will display information about the port, including a status of `Listen`.

If no information appears, the connector may not be running. See the [Troubleshooting Guide](TROUBLESHOOTING.md) for instructions on restarting it.

## Step 3 — Enable scripting in Affinity

Now that RunningHub for Affinity is installed, you can configure Affinity to communicate with it.

1. Open Affinity.
2. Go to **Edit → Settings → Scripting**.
3. Enable the following options:
   - **Enable Affinity Scripting**
   - **Access File System**
   - **Access networks**

Next, locate the **File System Access** section.

Add or enable access to the following folder:

`%LOCALAPPDATA%\RunningHubForAffinity\Exchange`

This permission allows Affinity to read and write the temporary images used by RunningHub for Affinity.

## Step 4 — Open the scripting tools

Affinity provides two tools for working with scripts.

**Script Editor**

Open it from:

`Window → Scripting → Script Editor`

This is where you paste and execute JavaScript code.

**Scripts Library**

Open it from:

`Window → Script → Scripts Library`

This allows you to save scripts and run them later without reopening their source files.

Keep both panels available for the following steps.

## Step 5 — Configure your RunningHub API Key

RunningHub identifies users through a personal credential called an **API Key**.

Each RunningHub user has their own API Key. It is private and must never be shared with other people.

To configure it, you will use the following script:

**`03_RunningHub_Settings.js`**

Location: `%LOCALAPPDATA%\RunningHubForAffinity\Bridge`

Follow these steps:

1. Open Windows File Explorer and navigate to the `Bridge` folder.
2. Open `03_RunningHub_Settings.js` in a text editor, such as Notepad.
3. Select all the code (`Ctrl+A`) and copy it (`Ctrl+C`).
4. Return to Affinity and paste the code into **Script Editor**.
5. Save the script to **Scripts Library**, using the suggested name:

   **RunningHub - Keys**

6. Run the script.
7. Enter your personal RunningHub API Key.
8. The settings interface includes a default AI App ID. Replace it with the **AI App ID** you want to use from your RunningHub account.
9. Save the configuration.

The API Key identifies your RunningHub account. The AI App ID identifies the specific AI application you want to run. They are different values.

## Step 6 — Add the Generate and Import script

The **Generate and Import** script sends content from Affinity to the RunningHub AI App or workflow configured in the previous step, then imports the generated result into Affinity.

Open the following file:

**`01_Generate_And_Import_Affinity.js`**

Location: `%LOCALAPPDATA%\RunningHubForAffinity\Bridge`

1. Open `01_Generate_And_Import_Affinity.js` using Notepad or another text editor.
2. Select all the code (`Ctrl+A`) and copy it (`Ctrl+C`).
3. Return to Affinity.
4. Open a new script in **Script Editor** and paste the copied code.
5. Save it to **Scripts Library** with the suggested name:

   **RunningHub - Generate and Import**

6. Your script is now ready to use.

## Step 7 — Start using RunningHub for Affinity

Once both scripts have been saved to Scripts Library, you can close them in Script Editor.

From now on, you can run them directly from **Scripts Library**, without copying and pasting their code again.

To generate your first result:

1. Open an image document in Affinity.
2. Select the layer of the image you want to process.
3. In Scripts Library, run **RunningHub - Generate and Import**.
4. Follow the instructions shown by the connector.
5. Wait for RunningHub to process the request and return the result to Affinity.

**Note — ComfyUI Workflows**

When using a ComfyUI workflow, RunningHub for Affinity may ask you to provide a Workflow API JSON file. You can download this file directly from RunningHub by opening the workflow's page and selecting **WorkFlow API**.

**Installation complete.** RunningHub for Affinity is now configured and ready to use.

---

**Important:** The installer does not automatically register the scripts inside Affinity. Do not copy `.template.js` files into Affinity.

**Tested:** Affinity 3.3.1 (Build 4940) on Windows. Other versions are unverified.

### Security and troubleshooting

Do not distribute your generated `.js` files. They embed your local connector token and may contain local Windows paths. Regenerate scripts on each computer by starting its own connector.

If Affinity displays **Invalid local token**, verify that you loaded scripts generated by the currently running connector on this computer, not older copies or templates.
