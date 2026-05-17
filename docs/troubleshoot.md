# Troubleshooting Log

> **Format:** Newest first, append-only

---

## 2026-04-24 — Extract Response Node Reading Wrong Data

**Symptoms:** Extract Response Code node in Gilgamesh hybrid routing workflow throwing errors when trying to process Claude API responses, showing undefined references.

**Root Cause:** After implementing merge node to combine Ollama and Claude routing paths, the Extract Response node was using generic `$json` reference instead of explicitly targeting the merged data from the correct source node.

**Resolution:** Updated Extract Response node to explicitly reference the Merge node output:
```javascript
// Before (failing):
const response = $json;

// After (working):
const response = $('Merge').first().json;
```

**Verification:** Tested hybrid routing with both simple and complex queries - Extract Response now correctly processes merged data from both routing paths.

**Lesson:** When using merge nodes or complex routing in n8n workflows, always explicitly reference the source node using `$('NodeName').first().json` instead of generic `$json` to ensure proper data flow.

---

## 2026-04-24 — httpRequestWithAuthentication Not Supported in Code Nodes

**Symptoms:** n8n Code node throwing error "httpRequestWithAuthentication is not a function" when attempting to make authenticated API calls to Claude within JavaScript code.

**Root Cause:** Code nodes in n8n have limited helper functions available. The `httpRequestWithAuthentication` method is not accessible within the Code node execution environment, only in specific node types.

**Resolution:** Replaced Code node API call with separate HTTP Request node configured with Claude API credential:
- Removed JavaScript API call from Code node
- Added dedicated HTTP Request node with Claude API authentication
- Connected nodes in proper sequence for data flow

**Verification:** Claude API calls now work correctly through dedicated HTTP Request node with proper credential management.

**Lesson:** n8n Code nodes have limited API access capabilities. Use dedicated HTTP Request nodes with credentials for authenticated external API calls instead of trying to implement them in JavaScript code.

---

## 2026-04-24 — SSH to VM 400 Wrong Password Error

**Symptoms:** n8n SSH connections to VM 400 (ollama-gpu) failing with "Authentication failed" error despite using stored root password.

**Root Cause:** VM 400 was configured with a regular user account (muzakkir) with sudo privileges, not direct root access. The root password was different from the user password, and SSH was configured to use the user account for security.

**Resolution:** Updated SSH configuration in n8n to use correct credentials:
- Username: muzakkir (not root)
- Password: User account password (different from root password)
- Commands requiring root: Use `sudo` prefix

**Verification:** SSH connection to VM 400 now successful with proper user authentication and sudo access for privileged commands.

**Lesson:** VM configurations may use non-root user accounts with sudo access for security. Always verify the actual user account configuration rather than assuming root access is available or uses the same password.

---

## 2026-04-24 — Open WebUI JSON Parse Error with NPM Proxy

**Symptoms:** Open WebUI showing JSON parsing errors when streaming responses through Nginx Proxy Manager. Browser console showing "Unexpected token" errors during model responses.

**Root Cause:** NPM default configuration includes proxy buffering, which interferes with streaming JSON responses from Ollama/Open WebUI. The buffering breaks the real-time streaming of JSON chunks during model inference.

**Resolution:** Added NPM advanced configuration for ollama.najhin-gaming.com proxy host:
```nginx
proxy_buffering off;
```
Added to Advanced tab → Custom Nginx Configuration

**Verification:** Open WebUI now streams responses correctly without JSON parsing errors. Model responses display in real-time without buffering delays.

**Lesson:** When proxying streaming applications (AI models, real-time APIs), disable proxy buffering in NPM advanced configuration to prevent breaking streaming JSON responses.

---

## 2026-04-24 — Open WebUI Not Accessible Externally

**Symptoms:** Open WebUI container running on VM 400 port 3000, but not accessible from external browsers via ollama.najhin-gaming.com. NPM proxy returning connection refused errors.

**Root Cause:** Ollama service was bound to localhost (127.0.0.1) only, preventing external container access. Default Ollama configuration doesn't allow network connections from other interfaces.

**Resolution:** Set environment variable `OLLAMA_HOST=0.0.0.0` to bind Ollama to all network interfaces:
```bash
echo "OLLAMA_HOST=0.0.0.0" >> ~/.bashrc
source ~/.bashrc
sudo systemctl restart ollama
```

**Verification:** Open WebUI now accessible via ollama.najhin-gaming.com. Docker container can successfully connect to Ollama API on all network interfaces.

**Lesson:** Ollama defaults to localhost-only access for security. Must explicitly bind to 0.0.0.0 for Docker container or external proxy access.

---

## 2026-04-24 — ROCm Not Detecting RX 6700 XT GPU

**Symptoms:** ROCm 6.1.3 installed but `rocminfo` showing "No ROCm device found" despite RX 6700 XT being passed through to VM successfully. GPU visible in `lspci` but not available for compute.

**Root Cause:** RX 6700 XT uses gfx10.3.0 architecture but ROCm was looking for explicitly supported GPU IDs. Navi 22 (RX 6700 XT) requires architecture override to be recognized by ROCm runtime.

**Resolution:** Set HSA environment variable to override GPU architecture detection:
```bash
echo "export HSA_OVERRIDE_GFX_VERSION=10.3.0" >> ~/.bashrc
source ~/.bashrc
```

**Verification:** `rocminfo` now shows RX 6700 XT as available compute device. Ollama successfully utilizing 100% GPU for model inference.

**Lesson:** Newer AMD GPUs may require architecture override (HSA_OVERRIDE_GFX_VERSION) for ROCm compatibility. Check AMD ROCm documentation for specific GPU architecture values.

---

## 2026-04-24 — vfio Kernel Modules Not Loading for GPU Passthrough

**Symptoms:** VM 400 failing to start with GPU passthrough enabled. Error: "vfio-pci device not found" despite RX 6700 XT being in correct IOMMU group and added to VM configuration.

**Root Cause:** vfio-pci module was not loading early enough in boot process. Default module loading order didn't ensure vfio modules were available before GPU initialization.

**Resolution:** Added softdep configuration to ensure proper module loading:
```bash
echo "softdep radeon pre: vfio-pci" >> /etc/modprobe.d/vfio.conf
echo "softdep amdgpu pre: vfio-pci" >> /etc/modprobe.d/vfio.conf
update-initramfs -u -k all
reboot
```

**Verification:** VM 400 starts successfully with RX 6700 XT passthrough. GPU visible in VM as primary graphics device.

**Lesson:** GPU passthrough requires vfio modules to load before native GPU drivers. Use softdep to control module loading order and update initramfs after changes.

---

## 2026-04-24 — VM Disk Space Insufficient for ROCm Installation

**Symptoms:** VM 400 running out of disk space during ROCm 6.1.3 installation. Error: "No space left on device" with df showing 100% usage on root filesystem.

**Root Cause:** Initial VM disk allocation of 20GB was insufficient for Ubuntu 22.04 + ROCm runtime + AI models. ROCm installation requires ~5GB, plus AI models (qwen2.5:14b = 8.7GB).

**Resolution:** Extended VM root disk from 20GB to 100GB:
```bash
# On Proxmox host
qm resize 400 scsi0 +80G

# Inside VM
sudo lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv
sudo resize2fs /dev/mapper/ubuntu--vg-ubuntu--lv
```

**Verification:** VM now has 100GB root disk with ~80GB free space, sufficient for ROCm + multiple AI models.

**Lesson:** AI/ML VMs require significant disk space for runtime environments and model storage. Plan 50GB+ for ROCm/CUDA + models.

---

## 2026-04-24 — Obsidian Dataview Query No Results

**Symptoms:** Dataview queries in Obsidian dashboard returning no results despite subscription notes existing in the vault.

**Root Cause:** Dashboard queries were targeting the same file containing the query instead of the folder containing individual subscription notes. Dataview requires separate note files for proper data querying.

**Resolution:** Created individual subscription note files in `04-personal/subscriptions/` folder:
- Each subscription as separate `.md` file (e.g., `claude-pro.md`, `netflix.md`)
- Dashboard queries target `"04-personal/subscriptions"` folder
- Each subscription note contains YAML frontmatter with cost, category, renewal date

**Verification:** Dashboard now displays subscription data with `TABLE cost, category, renewal FROM "04-personal/subscriptions"`

**Lesson:** Dataview plugin requires individual notes for database-style queries. Cannot query YAML frontmatter within the same file containing the query.

---

## 2026-04-24 — Nextcloud Sync Errors Due to Quota Exceeded

**Symptoms:** Obsidian vault sync failing with "quota exceeded" errors. Nextcloud showing sync errors and files not uploading to CT 220.

**Root Cause:** CT 220 root disk was 99% full (19.8GB used of 20GB). The original 20GB allocation was insufficient for Nextcloud data growth, user files, and system overhead.

**Resolution:** Resized CT 220 root disk from 20GB to 100GB:
```bash
pct resize 220 rootfs +80G
pct reboot 220
```

**Verification:** 
- Disk usage dropped from 99% to ~20%
- Nextcloud sync resumed successfully
- Obsidian vault files syncing normally

**Lesson:** Monitor container disk usage regularly. 20GB is insufficient for Nextcloud with active file syncing. Plan for growth when allocating container storage.

---

## 2026-04-24 — n8n Node Configuration Always Output Data

**Symptoms:** Get Alerts and Clear Memory DB nodes not proceeding to next nodes when empty results returned, causing workflow to terminate early.

**Root Cause:** n8n nodes with database queries or filters that return empty arrays/objects do not output data by default, preventing subsequent nodes from executing even when the empty result is a valid state.

**Resolution:** Enabled "Always Output Data" setting in both nodes:
- Get Alerts node: Always outputs array even when no alerts present
- Clear Memory DB node: Always outputs result even when no records found

**Verification:** Tested /alerts command with no active alerts - now returns "No active alerts" instead of no response. Tested /clear with empty memory - now returns confirmation message.

**Lesson:** For workflow reliability, enable "Always Output Data" on database/filter nodes where empty results are valid states that should proceed to subsequent steps.

---

## 2026-04-24 — n8n Format Alerts Array Handling Error

**Symptoms:** Format Alerts node showing "Active Alerts: undefined" when processing alert data from Alertmanager.

**Root Cause:** Code assumed alerts data would always be an array, but empty responses from Alertmanager can be undefined or null, causing array methods to fail.

**Resolution:** Added array validation in Code node:
```javascript
const alerts = inputData.alerts;
if (!Array.isArray(alerts) || alerts.length === 0) {
  return { alertText: "No active alerts" };
}
// Process alerts array...
```

**Lesson:** Always validate data types before using array/object methods in n8n Code nodes. External APIs can return varying data structures based on state.

---

## 2026-04-24 — n8n Format Cost Node JavaScript Syntax Error

**Symptoms:** Format Cost Code node failing with SyntaxError when processing token usage data for cost calculation display.

**Root Cause:** Code node contained JavaScript syntax that n8n couldn't parse - specifically using toLocaleString() method and emoji characters in template literals were causing parsing issues.

**Resolution:** Simplified JavaScript code in Format Cost node:
- Removed toLocaleString() formatting method
- Removed emoji characters from string templates  
- Used basic string concatenation instead of complex formatting

**Verification:** /cost command now displays token counts and estimated costs without syntax errors.

**Lesson:** Keep n8n Code node JavaScript simple - avoid advanced formatting methods and special characters that may not be supported in the execution environment.

---

## 2026