# Wont work as it is based on server core: https://github.com/microsoft/dotnet-framework-docker/blob/86c496ea02a7178d43a17300d2892b843aeb1f01/src/sdk/4.8/windowsservercore-ltsc2016/Dockerfile
#FROM mcr.microsoft.com/dotnet/framework/sdk:4.8 
# Developing on a Windows version which is incompatible... https://docs.microsoft.com/en-us/virtualization/windowscontainers/deploy-containers/version-compatibility?tabs=windows-server-20H2%2Cwindows-10-20H2
#FROM mcr.microsoft.com/windows:2004-amd64
FROM mcr.microsoft.com/windows:1903

# Normalize the shell, in case we switch to a Powershell shelled base image like framework/sdk once winget becomes baseline
SHELL ["cmd", "/S", "/C"]

WORKDIR C:/tools
WORKDIR C:/Downloads
RUN setx path "%path%;C:\tools"

# WinGet - VCRedist 
WORKDIR C:/Downloads
ADD https://aka.ms/vs/16/release/vc_redist.x64.exe C:/Downloads/vcredist_x64.exe
RUN C:/Downloads/vcredist_x64.exe /install /passive /norestart /log out.txt
ADD https://github.com/microsoft/winget-cli/releases/download/v0.2.2941/Microsoft.DesktopAppInstaller_8wekyb3d8bbwe.appxbundle C:/Downloads/Microsoft.DesktopAppInstaller.zip
RUN powershell -NoLogo -NonInteractive -NoProfile -ExecutionPolicy unrestricted -Command "Expand-Archive C:/Downloads/Microsoft.DesktopAppInstaller.zip C:/Downloads/ -Force"
RUN move AppInstaller_x64.appx winget.zip
RUN powershell -NoLogo -NonInteractive -NoProfile -ExecutionPolicy unrestricted -Command "Expand-Archive C:/Downloads/winget.zip C:/tools/ -Force"

# WinGet move exe into place
WORKDIR C:/tools
RUN move AppInstallerCLI.exe winget.exe


# .NET Core 3.1 SDK
RUN winget.exe install --exact Microsoft.dotnet --version 3.1.403

# Git
RUN winget.exe install --exact Git.Git --version 2.29.2.2

# NUKE
#RUN powershell -NoLogo -NonInteractive -NoProfile -ExecutionPolicy unrestricted -Command "dotnet tool install Nuke.GlobalTool --global"

#RUN git.exe


#.NET Core 3.1 SDK if we need newer than winget can provide
#WORKDIR C:/Downloads
#ADD https://dot.net/v1/dotnet-install.ps1 C:/Downloads/dotnet-install.ps1
#RUN powershell -NoLogo -NonInteractive -NoProfile -ExecutionPolicy unrestricted -Command "C:/Downloads/dotnet-install.ps1 -Channel 3.1"
