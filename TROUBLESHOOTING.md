# Troubleshooting and Exchange Folder

## Restart the connector

### Windows installer

1. Close any running `RunningHubConnector.exe` process if it is unresponsive (Task Manager).
2. Open File Explorer and enter:

   `%LOCALAPPDATA%\RunningHubForAffinity\Bridge`

3. Double-click `RunningHubConnector.exe`.
4. Return to Affinity and use the generated scripts from **this installation**.

The connector uses `127.0.0.1:8765`. Only one process can listen on that port at a time.

### Manual Python installation

Open Windows PowerShell:

```powershell
cd `%LOCALAPPDATA%\RunningHubForAffinity\Bridge`
py -3 .\conector.py
```

Leave the window open. Stop it with **Ctrl+C** before starting another instance.

## Exchange folder

The Exchange folder is normally located at:

`%LOCALAPPDATA%\RunningHubForAffinity\Exchange`

It stores temporary image exports, previews, and downloaded job results exchanged between Affinity and the connector.

**Do not delete the Exchange folder while a job is running, while a result is waiting to be recovered, or before you have saved important imported content.** Doing so can make recovery impossible.

To clean it safely:
1. Finish or cancel all active tasks and recover any pending results.
2. Save your Affinity documents and confirm the imported layers are present.
3. Close Affinity and stop the connector.
4. Delete **temporary contents** of Exchange, or remove the Exchange directory if you want it recreated on a subsequent job.
5. Restart the connector before continuing.

This cleanup is optional and is **not** part of normal installation.

## Common issues

| Symptom | Check |
|---|---|
| Connector not responding | Start/restart the connector; confirm nothing else is using port 8765 |
| Invalid local token | Use locally generated `.js` scripts, not `.template.js` or older copies |
| API Key not configured | Configure the key using the provided credential utility |
| Workflow discovery failed | Check API Key, AI App/Workflow ID, internet connectivity, and registration |
| Task still processing | Use the recovery script later; avoid resubmitting and spending credits |
| Missing images after Exchange cleanup | Temporary results may have been removed; recovery may no longer be possible |
| Script does not appear in Affinity | Enable scripting and add the generated script manually |

When reporting an issue, redact API Keys, local tokens, user-specific file paths, and private images.

## Recovering a completed RunningHub task

If a cloud job is still processing or the generated result was not imported into Affinity, **do not immediately submit the same job again**, as that may consume additional RunningHub credits. Once the task finishes, use the locally generated `02_Retrieve_Result_Affinity.js` script in Affinity to attempt recovery. Open it from the Bridge folder, copy its contents into Affinity Script Editor, and optionally save it to Scripts Library as **RunningHub - Retrieve Result**. Do not use the `.template.js` version.
