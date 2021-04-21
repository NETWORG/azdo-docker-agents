FROM mcr.microsoft.com/windows/servercore:ltsc2019

COPY start.ps1 .

# Install Visual Studio Components: https://docs.microsoft.com/en-us/visualstudio/install/workload-component-id-vs-enterprise?view=vs-2019
ADD https://aka.ms/vs/16/release/vs_community.exe "C:/Users/ContainerAdministrator/AppData/Local/Temp/vs_community.exe"
RUN "%TEMP%\vs_community.exe" \
    --add Microsoft.VisualStudio.Workload.NetWeb \
    --add Microsoft.VisualStudio.Workload.Node \
    --add Microsoft.VisualStudio.Workload.Azure \
    --add Microsoft.VisualStudio.Workload.ManagedDesktop \
    --add Microsoft.VisualStudio.Workload.NetCoreTools \
    --add Microsoft.NetCore.ComponentGroup.Web.2.1 \
    --add Microsoft.NetCore.ComponentGroup.DevelopmentTools.2.1 \
    --add Microsoft.Net.Core.Component.SDK.2.2 \
    --add Microsoft.Net.Core.Component.SDK.3.0 \
    --add Microsoft.Net.Component.4.6.1.SDK \
    --add Microsoft.Net.Component.4.6.2.SDK \
    --add Microsoft.Net.Component.4.7.1.SDK \
    --add Microsoft.Net.Component.4.7.2.SDK \
    --quiet --wait --norestart --noUpdateInstaller

### Check Visual Studio installation errors
RUN powershell -Command "if ($err = dir $Env:TEMP -Filter dd_setup_*_errors.log | where Length -gt 0 | Get-Content) { throw $err }"

### Cleanup Visual Studio installation
RUN \
rmdir /S /Q "%ProgramFiles(x86)%/Microsoft Visual Studio/Installer/" \
&& powershell Remove-Item -Force -Recurse "$Env:TEMP/*" \
&& rmdir /S /Q "%ProgramData%/Package Cache/"

# msbuild
RUN & 'C:/Program Files (x86)/Microsoft Visual Studio/2019/Community/MSBuild/Current/Bin/MSBuild.exe' /version

# choco
RUN powershell -Command "iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))"

# 7zip
RUN choco install -y 7zip.install

# # git
# RUN choco install -y git
# RUN git --version --build-options

# # nuget
# RUN choco install -y nuget.commandline
# RUN nuget help

# CMD ["powershell"]
# ENTRYPOINT ["C:\\Program Files (x86)\\Microsoft Visual Studio\\2019\\Community\\Common7\\Tools\\VsDevCmd.bat", "&&", "powershell.exe", "-NoLogo", "-ExecutionPolicy", "Bypass"]

CMD powershell .\start.ps1