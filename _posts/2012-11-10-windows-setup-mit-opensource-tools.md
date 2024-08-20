---
title: "Windows Setup Mit OpenSource Tools"
date: "2012-11-10"
categories: 
  - "entwicklung"
tags: 
  - "eclipse"
  - "nsis"
  - "windows-installer"
---

Vor kurzem hab ich im Büro ein kleines Tool geschrieben um binär serialisierte [Protobuf Dateien](http://code.google.com/p/protobuf/ "Protobuf - Googles data exchange protocol") von einem Gerät zu de-serialisieren. Schlussendlich wird das Tool dazu genutzt um Prüfdaten von einem Gerät in eine Textdatei zu schreiben.

Long story short: Das Tool sollte so schnell wie möglich an Kunden ausgeliefert werden, am besten als One-Click-No-Brain Download. Leider war unser InstallShield X was ich sonst für diverse Setups verwende aus Lizenzproblemen nicht verfügbar.

Ich hab mich dann kurzerhand für diverse OpenSource Software entschieden, um das Setup schnellst möglich liefern zu können

- [Nullsoft Scriptable Install System](http://nsis.sourceforge.net/Main_Page) (NSIS)
- [Eclipse](http://www.eclipse.org/downloads/)
- [EclipseNSIS Plugin](http://eclipsensis.sourceforge.net/index.shtml) (Vereinfacht NSIS debugging, testing, GUI creation)
- [7Zip](http://www.7-zip.de/) 
- [7Zip Sfx Maker](http://sourceforge.net/projects/sfx-maker/)

Letztendlich ist es mir gelungen ohne das ich die Tools vorher kannte relativ zügig ein kleines Setup mit Hilfe von Eclipse und NSIS zusammen zustellen. Gut ok ... kleines bissl Hintergrundwissen von InstallShield - Aber an sich ist NSIS recht angenehm zu handeln.

Ich hab in meinen Installer noch eine Funktion eingebaut die das OS auf  vorhandene .NET Versionen überprüft - Das Tool das installiert werden sollte benötigt .NET 4. Falls .NET 4 nicht mindestens in der entsprechenden Version auf dem OS vorhanden ist, wird es vom Setup installiert.

Vollständiges NSIS Template:

```
# NSIS Installer Script
# Template by nschoenberg 09.11.2012 07:20:15

Name "Your Product Name"

# General Symbol Definitions
!define REGKEY "SOFTWARE\$(^Name)"
!define VERSION 1.0.0.4
!define COMPANY "Your company"
!define URL http://blog.grrbrr.de

# MUI Symbol Definitions
!define MUI_ICON "${NSISDIR}\Contrib\Graphics\Icons\modern-install-blue.ico"
!define MUI_FINISHPAGE_NOAUTOCLOSE
!define MUI_STARTMENUPAGE_REGISTRY_ROOT HKLM
!define MUI_STARTMENUPAGE_REGISTRY_KEY ${REGKEY}
!define MUI_STARTMENUPAGE_REGISTRY_VALUENAME StartMenuGroup
!define MUI_STARTMENUPAGE_DEFAULTFOLDER "MSA"
!define MUI_UNICON "${NSISDIR}\Contrib\Graphics\Icons\modern-uninstall-blue.ico"
!define MUI_UNFINISHPAGE_NOAUTOCLOSE

# .NET requirements
# Change this values for the desired .Net Version
!define DOT_MAJOR "4"
!define DOT_MINOR "0"
!define DOT_MINOR_MINOR "30319"

# Included files
!include Sections.nsh
!include MUI2.nsh

# Reserved Files
ReserveFile "${NSISDIR}\Plugins\AdvSplash.dll"

# Variables
Var StartMenuGroup

# Installer pages
!insertmacro MUI_PAGE_WELCOME
!insertmacro MUI_PAGE_DIRECTORY
!insertmacro MUI_PAGE_STARTMENU Application $StartMenuGroup
!insertmacro MUI_PAGE_INSTFILES
!insertmacro MUI_PAGE_FINISH
!insertmacro MUI_UNPAGE_CONFIRM
!insertmacro MUI_UNPAGE_INSTFILES

# Installer languages
!insertmacro MUI_LANGUAGE English

# Installer attributes
OutFile setup.exe
InstallDir "$PROGRAMFILES\YourProductInstallationPath\"
CRCCheck on
XPStyle on
ShowInstDetails show
VIProductVersion 1.0.0.4
VIAddVersionKey ProductName "My awesome Product"
VIAddVersionKey ProductVersion "${VERSION}"
VIAddVersionKey CompanyName "${COMPANY}"
VIAddVersionKey CompanyWebsite "${URL}"
VIAddVersionKey FileVersion "${VERSION}"
VIAddVersionKey FileDescription ""
VIAddVersionKey LegalCopyright ""
InstallDirRegKey HKLM "${REGKEY}" Path
ShowUninstDetails show

# Installer sections
Section -Main SEC0000
SetOutPath $INSTDIR
SetOverwrite on
# Add all your files you want to install here
File "path to file\your_file.exe"
File "path to file\ico.ico"

WriteRegStr HKLM "${REGKEY}\Components" Main 1

SectionEnd

Section -post SEC0001
WriteRegStr HKLM "${REGKEY}" Path $INSTDIR
SetOutPath $INSTDIR
WriteUninstaller $INSTDIR\uninstall.exe
!insertmacro MUI_STARTMENU_WRITE_BEGIN Application
CreateShortCut "$DESKTOP\My Shortcut.lnk" "$INSTDIR\file.exe" "" "$INSTDIR\icon.ico"
CreateDirectory "$SMPROGRAMS\$StartMenuGroup\My Awesome Product"
CreateShortCut "$SMPROGRAMS\$StartMenuGroup\My Awesome Product\My Shortuct.lnk" "$INSTDIR\file.exe" "" "$INSTDIR\icon.ico"
!insertmacro MUI_STARTMENU_WRITE_END
WriteRegStr HKLM "SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\$(^Name)" DisplayName "$(^Name)"
WriteRegStr HKLM "SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\$(^Name)" DisplayVersion "${VERSION}"
WriteRegStr HKLM "SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\$(^Name)" Publisher "${COMPANY}"
WriteRegStr HKLM "SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\$(^Name)" URLInfoAbout "${URL}"
WriteRegStr HKLM "SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\$(^Name)" DisplayIcon $INSTDIR\uninstall.exe
WriteRegStr HKLM "SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\$(^Name)" UninstallString $INSTDIR\uninstall.exe
WriteRegDWORD HKLM "SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\$(^Name)" NoModify 1
WriteRegDWORD HKLM "SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\$(^Name)" NoRepair 1
SectionEnd

# Macro for selecting uninstaller sections
!macro SELECT_UNSECTION SECTION_NAME UNSECTION_ID
Push $R0
ReadRegStr $R0 HKLM "${REGKEY}\Components" "${SECTION_NAME}"
StrCmp $R0 1 0 next${UNSECTION_ID}
!insertmacro SelectSection "${UNSECTION_ID}"
GoTo done${UNSECTION_ID}
next${UNSECTION_ID}:
!insertmacro UnselectSection "${UNSECTION_ID}"
done${UNSECTION_ID}:
Pop $R0
!macroend

# Uninstaller sections
Section /o -un.Main UNSEC0000
# delete all your installed files here
Delete "path to file\your_file.exe"
Delete "path to file\ico.ico"

!insertmacro MUI_STARTMENU_GETFOLDER Application $StartMenuGroup
Delete "$SMPROGRAMS\$StartMenuGroup\Galaxy Gx2 Sd Card Reader\Galaxy Gx2 Sd Card Reader.lnk"
RMDir "$SMPROGRAMS\$StartMenuGroup\Galaxy Gx2 Sd Card Reader"
Delete "$DESKTOP\Galaxy Gx2 Sd Card Reader.lnk"
DeleteRegValue HKLM "${REGKEY}\Components" Main
SectionEnd

Section -un.post UNSEC0001
DeleteRegKey HKLM "SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\$(^Name)"
Delete /REBOOTOK $INSTDIR\uninstall.exe
DeleteRegValue HKLM "${REGKEY}" StartMenuGroup
DeleteRegValue HKLM "${REGKEY}" Path
DeleteRegKey /IfEmpty HKLM "${REGKEY}\Components"
DeleteRegKey /IfEmpty HKLM "${REGKEY}"
RmDir /REBOOTOK $SMPROGRAMS\$StartMenuGroup
RmDir /REBOOTOK $INSTDIR
Push $R0
StrCpy $R0 $StartMenuGroup 1
StrCmp $R0 ">" no_smgroup
no_smgroup:
Pop $R0
SectionEnd

# Installer functions

; Usage
; Define in your script two constants:
; DOT_MAJOR "(Major framework version)"
; DOT_MINOR "{Minor framework version)"
; DOT_MINOR_MINOR "{Minor framework version – last number after the second dot)"
;
; Call IsDotNetInstalledAdv
; This function will abort the installation if the required version
; or higher version of the .NET Framework is not installed. Place it in
; either your .onInit function or your first install section before
; other code.
Function IsDotNetInstalledAdv
Push $0
Push $1
Push $2
Push $3
Push $4
Push $5

StrCpy $0 "0"
StrCpy $1 "SOFTWARE\Microsoft\.NETFramework" ;registry entry to look in.
StrCpy $2 0

StartEnum:
;Enumerate the versions installed.
EnumRegKey $3 HKLM "$1\policy" $2

;If we don’t find any versions installed, it’s not here.
StrCmp $3 "" noDotNet notEmpty

;We found something.
notEmpty:
;Find out if the RegKey starts with ‘v’.
;If it doesn’t, goto the next key.
StrCpy $4 $3 1 0
StrCmp $4 "v" +1 goNext
StrCpy $4 $3 1 1

;It starts with ‘v’. Now check to see how the installed major version
;relates to our required major version.
;If it’s equal check the minor version, if it’s greater,
;we found a good RegKey.
IntCmp $4 ${DOT_MAJOR} +1 goNext yesDotNetReg
;Check the minor version. If it’s equal or greater to our requested
;version then we’re good.
StrCpy $4 $3 1 3
IntCmp $4 ${DOT_MINOR} +1 goNext yesDotNetReg

;detect sub-version – e.g. 2.0.50727
;takes a value of the registry subkey – it contains the small version number
EnumRegValue $5 HKLM "$1\policy\$3" 0

IntCmpU $5 ${DOT_MINOR_MINOR} yesDotNetReg goNext yesDotNetReg

goNext:
;Go to the next RegKey.
IntOp $2 $2 + 1
goto StartEnum

yesDotNetReg:
;Now that we’ve found a good RegKey, let’s make sure it’s actually
;installed by getting the install path and checking to see if the
;mscorlib.dll exists.
EnumRegValue $2 HKLM "$1\policy\$3" 0
;$2 should equal whatever comes after the major and minor versions
;(ie, v1.1.4322)
StrCmp $2 "" noDotNet
ReadRegStr $4 HKLM $1 "InstallRoot"
;Hopefully the install root isn’t empty.
StrCmp $4 "" noDotNet
;build the actuall directory path to mscorlib.dll.
StrCpy $4 "$4$3.$2\mscorlib.dll"
IfFileExists $4 yesDotNet noDotNet

noDotNet:
;Nope, something went wrong along the way. Looks like the
;proper .NET Framework isn’t installed.

;Uncomment the following line to make this function throw a message box right away
; MessageBox MB_OK "You must have v${DOT_MAJOR}.${DOT_MINOR}.${DOT_MINOR_MINOR} or greater of the .NET Framework installed. Aborting!"
; Abort
StrCpy $0 0
Goto done

yesDotNet:
;Everything checks out. Go on with the rest of the installation.
StrCpy $0 1

done:
Pop $4
Pop $3
Pop $2
Pop $1
Exch $0
FunctionEnd

Function .onInit
InitPluginsDir
Push $R1

File /oname=$PLUGINSDIR\spltmp.bmp images\logo.bmp
advsplash::show 1000 600 400 -1 $PLUGINSDIR\spltmp
call IsDotNetInstalledAdv
Pop $0

IntCmp $0 1 goOnWithSetup askDoInstallnet
askDoInstallnet:
${If} ${Cmd} ‘MessageBox MB_YESNO "You must have v${DOT_MAJOR}.${DOT_MINOR}.${DOT_MINOR_MINOR} or greater of the .NET Framework installed to proceed with the Setup. Do you want to install it now?" IDYES’
goto doInstallNet
${Else}
Abort
${EndIf}
doInstallNet:
ExecWait "$EXEDIR\net40\dotNetFx40_Full_x86_x64.exe"
goto goOnWithSetup
goOnWithSetup:
Pop $R1
Pop $R1
FunctionEnd

# Uninstaller functions
Function un.onInit
ReadRegStr $INSTDIR HKLM "${REGKEY}" Path
!insertmacro MUI_STARTMENU_GETFOLDER Application $StartMenuGroup
!insertmacro SELECT_UNSECTION Main ${UNSEC0000}
FunctionEnd
```

Nachdem das Setup fertig war, habe ich mit Hilfe von 7-Zip und 7 zip Sfx maker ein Selbstextrahierendes-Archiv erstellt das nachdem entpacken automatisch das Setup startet.

Im Prinzip braucht man den 7 Zip Sfx Maker nicht, da einfach 7-Zip mit bestimmten Parametern aufgerufen wird. Aber wenn man Faul ist oder Zeitdruck hat kann ich den 7 Zip Sfx Maker nur empfehlen. Einfach vom gewünschten Inhalt (In meinem Fall das komplette Setup und dazugehörige Dateien) als 7Zip-Archiv komprimieren, in den 7 Zip Sfx Maker reinladen und fertig.
