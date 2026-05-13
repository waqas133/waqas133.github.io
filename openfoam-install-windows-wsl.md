# Install OpenFOAM 13 on Windows 10/11 using WSL

Beginner-friendly installation notes for installing OpenFOAM 13 on Windows 10/11 using WSL and Ubuntu 22.04.

## Before starting: which terminal should you use?

**PowerShell** is the Windows terminal used here for WSL installation commands. Open it from Windows as Administrator.

**CMD** is also a Windows terminal, but it is different from PowerShell. In these notes, use PowerShell whenever a step says **PowerShell**.

**Ubuntu terminal / Bash** is the Linux terminal inside WSL. After Ubuntu is installed, open it from the Start menu or by running this command from PowerShell:

```bash
wsl -d Ubuntu-22.04
```

## A) Windows side — PowerShell as Administrator

### 1. Install WSL with Ubuntu 22.04

**Use:** PowerShell as Administrator

```powershell
wsl --install -d Ubuntu-22.04
```

This installs WSL and Ubuntu 22.04.

Restart Windows after the installation if requested.

After the installation, Ubuntu may ask you to create a Linux username and password. This username and password are for Ubuntu/WSL and do not need to be the same as your Windows login.

## B) Linux side — Ubuntu terminal / Bash

### 2. Check Ubuntu version

**Use:** Ubuntu terminal / Bash

```bash
lsb_release -a
```

This confirms that Ubuntu 22.04 is installed.

### 3. Update Ubuntu

**Use:** Ubuntu terminal / Bash

```bash
sudo apt update && sudo apt upgrade -y
```

This refreshes the package information and installs available updates.

### 4. Install required helper tools

**Use:** Ubuntu terminal / Bash

```bash
sudo apt install -y wget software-properties-common
```

This installs the tools needed to add the OpenFOAM repository.

### 5. Add the OpenFOAM public key

**Use:** Ubuntu terminal / Bash

```bash
sudo sh -c "wget -O - https://dl.openfoam.org/gpg.key > /etc/apt/trusted.gpg.d/openfoam.asc"
```

This allows Ubuntu to verify OpenFOAM packages.

### 6. Add the OpenFOAM repository

**Use:** Ubuntu terminal / Bash

```bash
sudo add-apt-repository http://dl.openfoam.org/ubuntu
```

This tells Ubuntu where to download OpenFOAM from.

### 7. Update the package list again

**Use:** Ubuntu terminal / Bash

```bash
sudo apt update
```

This loads the package list from the OpenFOAM repository.

### 8. Install OpenFOAM 13 and ParaView

**Use:** Ubuntu terminal / Bash

```bash
sudo apt install -y openfoam13
```

This installs OpenFOAM 13. ParaView is usually installed as a dependency.

### 9. Add OpenFOAM to `.bashrc`

**Use:** Ubuntu terminal / Bash

```bash
notepad.exe ~/.bashrc
```

This opens the `.bashrc` file with Windows Notepad.

Add this line at the bottom of the `.bashrc` file:

```bash
. /opt/openfoam13/etc/bashrc
```

This activates OpenFOAM automatically whenever the Ubuntu terminal starts.

You can also open `.bashrc` directly using any text editor. The general location of this file is:

```bash
/home/<your-ubuntu-username>/.bashrc
```

If you do not see the file in this folder, enable **Show hidden files**, because files starting with a dot are hidden by default.

After saving the `.bashrc` file, close and reopen the Ubuntu terminal.

### 10. Optional: create aliases for different OpenFOAM versions

Aliases are useful when more than one OpenFOAM version is installed on the same system.

Add alias lines at the bottom of the same `.bashrc` file, for example:

```bash
alias of13='source /opt/openfoam13/etc/bashrc'
alias of12='source /opt/openfoam12/etc/bashrc'
```

This lets you quickly switch to a specific OpenFOAM version by typing the corresponding alias in the Ubuntu terminal.

For example:

```bash
of13
```

After saving the `.bashrc` file, close and reopen the Ubuntu terminal.

### 11. Optional: select the correct GPU for ParaView

If ParaView crashes or behaves unexpectedly, the issue may be related to graphics adapter selection, especially on systems with more than one GPU.

In that case, WSL graphical applications can be instructed to use a specific graphics adapter by adding the following line at the bottom of the same `.bashrc` file:

```bash
export MESA_D3D12_DEFAULT_ADAPTER_NAME=<GPU_NAME>
```

Replace `<GPU_NAME>` with a suitable part of your graphics adapter name, for example `NVIDIA`, `AMD`, `Radeon`, or `Intel`, depending on your system.

Example:

```bash
export MESA_D3D12_DEFAULT_ADAPTER_NAME=NVIDIA
```

This can help ParaView use the correct graphics adapter instead of the default one selected automatically by WSL.

After saving the `.bashrc` file, close and reopen the Ubuntu terminal before starting ParaView again.
