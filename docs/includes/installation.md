Download and install the latest pre-compiled binary for your operating system from our [**GitHub Releases page**](https://github.com/flowcontextually/cx-shell/releases).

**Linux**

```bash
# This script downloads the latest Linux binary, extracts it, and moves it to your path.
curl -sL https://github.com/flowcontextually/cx-shell/releases/download/v0.1.0/cx-v0.1.0-linux-x86_64.tar.gz | tar -xz
sudo mv cx /usr/local/bin/
cx --version
```

**macOS (Intel)**

```bash
# This script downloads the latest macOS (Intel) binary, extracts it, and moves it to your path.
curl -sL https://github.com/flowcontextually/cx-shell/releases/download/v0.1.0/cx-v0.1.0-macos-x86_64.tar.gz | tar -xz
sudo mv cx /usr/local/bin/
cx --version
```

**macOS (Apple Silicon)**

> **Warning:** A native Apple Silicon build is not yet available. You can run the Intel version via Rosetta 2 using the macOS (Intel) instructions.

**Windows (PowerShell)**

```powershell
# This script downloads the latest Windows binary and unzips it.
$url = "https://github.com/flowcontextually/cx-shell/releases/download/v0.1.0/cx-v0.1.0-windows-amd64.zip"
$output = "cx.zip"
Invoke-WebRequest -Uri $url -OutFile $output
Expand-Archive -Path $output -DestinationPath .

# You should now have `cx.exe` in the current directory.
# For system-wide access, move `cx.exe` to a directory in your system's PATH.
./cx.exe --version
```
