# RunningHub for Affinity — Manual Installation (Windows)

This guide explains how to install RunningHub for Affinity manually, without using the Windows installer.

**You will need Python.** You do not need to know how to program: follow the steps in order and copy the commands exactly as shown.

## Step 1 — Install Python

Python is the program that runs the RunningHub connector in this installation method.

1. Download Python for Windows from [python.org/downloads/windows](https://www.python.org/downloads/windows/). **Python 3.12 is recommended for this release.**
2. Run the Python installer. If offered, enable **Add Python to PATH**, then complete the installation.
3. Open **Windows PowerShell**: open the Windows Start menu, type `PowerShell`, and select **Windows PowerShell**.
4. Copy and run this command:

   ```powershell
   py -3 --version
   ```

5. If Python is installed correctly, PowerShell will show a Python version (for example, `Python 3.12.x`). If you see an error, finish setting up Python before continuing.

## Step 2 — Download the manual package

1. Open the project's **GitHub Releases** page.
2. Download `RunningHub-for-Affinity-v1.0.0-Manual.zip`.
3. Right-click the downloaded ZIP and select **Extract All**.
4. Move the extracted **Bridge** folder to `%LOCALAPPDATA%\RunningHubForAffinity\Bridge`.

After this step, the following file should exist:

`%LOCALAPPDATA%\RunningHubForAffinity\Bridge\conector.py`

**Important:** The manual ZIP contains the files needed to run the connector. Do not download the source-code ZIP from GitHub instead of the manual installation package.

## Step 3 — Understand the folders

The manual installation uses these locations:

- **Bridge** — `%LOCALAPPDATA%\RunningHubForAffinity\Bridge`: contains the connector and the scripts that it generates for your computer.
- **Exchange** — `%LOCALAPPDATA%\RunningHubForAffinity\Exchange`: contains temporary images and results exchanged with Affinity. The connector creates this folder when it starts.
- **Preferences** — the manual package does not require you to create this folder. Configuration files are stored in your **Bridge** folder.

You do not need to create the Exchange folder yourself. Keep the Bridge folder in its chosen location after configuring Affinity.

## Step 4 — Start the connector

The **connector** is the program that communicates between Affinity and RunningHub. In manual installation, **you start it yourself** and keep its PowerShell window open while working.

1. Open **Windows PowerShell**.
2. Copy and run the following command to open the Bridge folder:

   ```powershell
   cd "$env:LOCALAPPDATA\RunningHubForAffinity\Bridge"
   ```

3. Start the connector:

   ```powershell
   py -3 .\conector.py
   ```

   **API Key (optional at this step):** If the connector asks for your RunningHub API Key, you can enter it now or press `Enter` to skip. You can configure your API Key later in **Step 7 — Configure your RunningHub API Key** using `03_RunningHub_Settings.js`. A valid API Key is required before submitting jobs to RunningHub.

4. Wait until the window shows that the connector is running. The connector also generates three `.js` files in the Bridge folder:
   - `01_Generate_And_Import_Affinity.js`
   - `02_Retrieve_Result_Affinity.js`
   - `03_RunningHub_Settings.js`
5. **Leave this PowerShell window open.** Closing it stops the connector.

The connector uses local port **8765** to communicate with Affinity. To check it, open a **second** PowerShell window and run:

```powershell
Get-NetTCPConnection -LocalPort 8765 -State Listen -ErrorAction SilentlyContinue
```

If the connector is running, you should see an entry with a status of `Listen`. If nothing appears, or the first PowerShell window shows an error, see the [Troubleshooting Guide](TROUBLESHOOTING.md).

## Step 5 — Enable scripting in Affinity

Now that the connector is running and the Exchange folder exists, configure Affinity.

1. Open Affinity.
2. Go to **Edit → Settings → Scripting**.
3. Enable:
   - **Enable Affinity Scripting**
   - **Access File System**
   - **Access networks**
4. Under **File System Access**, add or enable access to:

   `%LOCALAPPDATA%\RunningHubForAffinity\Exchange`

This permission allows Affinity to read and write the temporary images used by RunningHub for Affinity. If Affinity does not accept `%LOCALAPPDATA%` in its folder picker, paste that path into the Windows File Explorer address bar first and select the actual **Exchange** folder.

## Step 6 — Open the scripting tools

Affinity provides two tools:

**Script Editor** — Open it from `Window → Scripting → Script Editor`. This is where you paste and execute JavaScript code.

**Scripts Library** — Open it from `Window → Script → Scripts Library`. This allows you to save scripts and run them later without reopening their source files.

Keep both panels available for the following steps.

## Step 7 — Configure your RunningHub API Key

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

## Step 8 — Add the Generate and Import script

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

## Step 9 — Start using RunningHub for Affinity

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

**Important:** Do not copy `.template.js` files into Affinity. Use only the `.js` scripts generated by the connector on your own computer.

**Tested:** Affinity 3.3.1 (Build 4940) on Windows. Other versions are unverified.

**Security and troubleshooting**

Do not distribute your generated `.js` files. They embed your local connector token and may contain local Windows paths. Regenerate scripts on each computer by starting its own connector.

If Affinity displays **Invalid local token**, verify that you loaded scripts generated by the currently running connector on this computer, not older copies or templates.
