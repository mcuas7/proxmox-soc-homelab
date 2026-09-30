# Investigation 02: File integrity monitoring in Wazuh

**Date:** September 30, 2026  
**Status:** Creation, modification, and deletion verified.

## What I wanted to test

For my second investigation, I wanted to see how Wazuh detects changes to a file on my Debian lab desktop. I used a dedicated test directory and harmless text so I could compare my own actions with the alerts and file differences recorded by Wazuh.

## Lab and configuration

- Endpoint: `lab-desktop`, agent ID `001`, IP `192.168.1.143` in the alert.
- Wazuh manager: `soc-wazuh`.
- Test file: `/home/marco/fim-lab/test-file.txt`.

I created `/home/marco/fim-lab` and added this entry inside the existing `<syscheck>` section of `/var/ossec/etc/ossec.conf` on the Debian endpoint:

```xml
<directories realtime="yes" report_changes="yes" check_all="yes">/home/marco/fim-lab</directories>
```

After saving the configuration and restarting the agent, I checked its log for the monitored directory and completion of the initial FIM scan. Saving the configuration required some troubleshooting because the browser intercepted editor shortcuts. I confirmed the file was saved before continuing.

The terminal evidence below confirms the saved configuration entry, an active agent after restart, the monitored directory, and completion of the initial scan. The dashboard evidence shows the resulting alerts.

## Test actions

I created the file with:

```bash
echo "SOC lab: original content" > /home/marco/fim-lab/test-file.txt
```

Then I appended a second line:

```bash
echo "SOC lab: this line was added during the modification test" >> /home/marco/fim-lab/test-file.txt
```

In Wazuh Threat Hunting, I searched for:

```text
agent.name:"lab-desktop" AND syscheck.path:"/home/marco/fim-lab/test-file.txt"
```

## Evidence and timeline

Times below are displayed by the dashboard on September 30, 2026. The screenshots do not show its timezone setting.

| Displayed time | Evidence | Result |
| --- | --- | --- |
| 11:57:41 (fraction truncated) | Rule 554, level 5 | File added to the system. |
| 12:05:06.000 | `syscheck.mtime_after` | Modification time recorded for the file. |
| 12:05:07.264 | Rule 550, level 7 | Integrity checksum changed. |

### 1. Creation and modification alerts

The filtered event list contains two hits for my test file: the creation alert and the later modification alert.

![Creation and modification alerts](images/02-file-integrity/evidence-01.png)

### 2. Endpoint and original event details

The document identifies agent `001`, `lab-desktop`, and decoder `syscheck_integrity_changed`. The visible original log names my test file, identifies real-time mode, and reports a size change from 26 to 84 bytes.

![Endpoint and modification log](images/02-file-integrity/evidence-02.png)

### 3. Rule and classification

Rule 550 has severity level 7. The event lists changed attributes `size, mtime, md5, sha1, sha256`. It also carries MITRE ATT&CK mapping `T1565.001`, Stored Data Manipulation, under Impact. This mapping describes a category of behavior; it does not establish malicious activity in this test.

![Rule details and changed attributes](images/02-file-integrity/evidence-03.png)

### 4. Exact content change

The event records `syscheck.event: modified` and `syscheck.mode: realtime`. The difference shows the line I deliberately appended:

```diff
1a2
> SOC lab: this line was added during the modification test
```

Before and after MD5 and SHA-1 values differ. The screenshot also shows the SHA-256 after value and the test file path. This connects the alert to the specific content change rather than relying on the rule title alone.

![Content difference and file attributes](images/02-file-integrity/evidence-04.png)

### 5. Size and event timestamp

The final screenshot shows the SHA-256 before field, file size increasing from 26 to 84 bytes, owner `marco` (UID 1000), and event timestamp `12:05:07.264`. File ownership alone does not identify which process or person performed a change.

![File size and event timestamp](images/02-file-integrity/evidence-05.png)

### 6. Deletion log and endpoint

After the modification test, I proceeded with the deletion test for the same file. Wazuh recorded decoder `syscheck_deleted` on agent `001`, `lab-desktop`. The original event states:

```text
File '/home/marco/fim-lab/test-file.txt' deleted
Mode: realtime
```

![Deletion log and endpoint](images/02-file-integrity/evidence-06.png)

### 7. Deletion rule and file details

The deletion event shows rule `553`, level `7`, `syscheck.event: deleted`, `syscheck.mode: realtime`, and the same test file path. Its displayed MD5 matches the modified file's MD5 from the earlier event, providing another connection between the two records.

The rule carries mappings `T1070.004` (File Deletion) and `T1485` (Data Destruction). These are rule classifications, not evidence that my controlled deletion was an attack.

The screenshot shows `syscheck.mtime_after` of `12:05:06.000`, which is the file's recorded modification time, not the deletion alert timestamp. The deletion alert timestamp is outside the captured area, so I have not assigned an exact deletion time in the timeline.

![Deletion rule and file details](images/02-file-integrity/evidence-07.png)

### 8. Debian configuration, agent status, and test commands

The terminal screenshot shows the saved directory entry at line 107 inside `<syscheck>`, with `<disabled>no</disabled>`. After restarting `wazuh-agent`, `systemctl is-active wazuh-agent` returns `active`. At 11:54:43, the agent log identifies `/home/marco/fim-lab` as a monitored directory with real-time monitoring and content-change reporting; the scan ends at 11:54:45.

The same terminal shows creation at `11:57:40 AM EDT` and the append command at `12:05:06 PM EDT` on September 30, 2026. These align with the dashboard's creation and modification records.

Earlier command errors show instructional text accidentally pasted into the shell. The initial configuration search returned no match; a later search confirms the saved entry. The log also includes older connection errors at 10:53–10:54. Those historical lines do not establish a continuing outage: the later dashboard screenshots demonstrate successful alert delivery.

![Debian configuration and creation and modification commands](images/02-file-integrity/evidence-08.png)

### 9. Debian deletion and verification

The second terminal screenshot displays both expected lines in the file. A `date` command immediately before the deletion sequence shows `12:15:07 PM EDT`. I then ran:

```bash
rm /home/marco/fim-lab/test-file.txt
ls -l /home/marco/fim-lab/test-file.txt
```

The check returns `No such file or directory`, confirming that the path was absent after deletion. The displayed time is a pre-deletion terminal check, not the exact deletion or alert timestamp. Combined with rule 553 and `syscheck.event: deleted`, this connects my local test action to Wazuh's detection.

![Debian deletion command and verification](images/02-file-integrity/evidence-09.png)

## Conclusion

Wazuh detected the creation, modification, and deletion of my test file. The modification alert matches my planned action: the correct endpoint and path, a 58-byte increase, changed hashes, and the exact appended line.

This is expected, benign lab activity with a valid detection. The alert severity and MITRE mapping do not make it a confirmed security incident. In a real investigation, I would also check whether the change was authorized and gather process or audit evidence to establish who made it.

## What I learned

I verified the full create–modify–delete sequence using rules 554, 550, and 553. The most useful evidence was the matching endpoint and file path, the exact content difference, the size and hash changes, and the explicit deleted event. I also learned to distinguish a file modification timestamp from an alert timestamp and to avoid treating a MITRE mapping as proof of malicious intent.

All nine supplied screenshots are preserved unchanged. The terminal evidence now covers configuration, creation, modification, and deletion. The exact deletion alert timestamp remains outside the captured dashboard area.
