---
layout: post
categories: script
---

It's a summary of the article [Show a popup/message box from a Windows batch file](http://stackoverflow.com/questions/774175/show-a-popup-message-box-from-a-windows-batch-file) from [StackOverflow](http://stackoverflow.com/).

## VBS

Something like the following saved in `MessageBox.vbs`:

```
Set objArgs = WScript.Arguments
messageText = objArgs(0)
MsgBox messageText
```

Call like:

```
cscript MessageBox.vbs "This will be shown in a popup."
```

## VBS in Batch

This way your batch file will create a VBS script and show a popup. After it runs, the batch file will delete that intermediate file.

The advantage of using MSGBOX is that it is really customaziable (change the title, the icon etc) while MSG.exe isn't as much.

```
echo MSGBOX "YOUR MESSAGE" > %temp%\TEMPmessage.vbs
call %temp%\TEMPmessage.vbs
del %temp%\TEMPmessage.vbs /f /q
```

## Mshta

```
mshta javascript:alert("Message\n\nMultiple\nLines\ntoo!");close();
```

Don't forget to escape your parentheses if you're using if:

```
if 1 == 1 (
   mshta javascript:alert^("1 is equal to 1, amazing."^);close^(^);
   )
```

Another one:

```
@if (true == false) @end /*!
@echo off
mshta "about:<script src='file://%~f0'></script><script>close()</script>" %*
goto :EOF */

alert("Hello, world!");
```


## Msg

```
Msg * "insert your message here" 
```

## Command Prompt

```
START CMD /C "ECHO My Popup Message && PAUSE"
```

```
start "" cmd /c "echo(&echo(&echo              Hello world!     &echo(&pause>nul"
```

## Geeker's Way

```
;@echo off
;setlocal

;set ppopup_executable=popupe.exe
;set "message2=click OK to continue"
;
;del /q /f %tmp%\yes >nul 2>&1
;
;copy /y "%~f0" "%temp%\popup.sed" >nul 2>&1

;(echo(FinishMessage=%message2%)>>"%temp%\popup.sed";
;(echo(TargetName=%cd%\%ppopup_executable%)>>"%temp%\popup.sed";
;(echo(FriendlyName=%message1_title%)>>"%temp%\popup.sed"
;
;iexpress /n /q /m %temp%\popup.sed
;%ppopup_executable%
;rem del /q /f %ppopup_executable% >nul 2>&1

;pause

;endlocal
;exit /b 0


[Version]
Class=IEXPRESS
SEDVersion=3
[Options]
PackagePurpose=InstallApp
ShowInstallProgramWindow=1
HideExtractAnimation=1
UseLongFileName=0
InsideCompressed=0
CAB_FixedSize=0
CAB_ResvCodeSigning=0
RebootMode=N
InstallPrompt=%InstallPrompt%
DisplayLicense=%DisplayLicense%
FinishMessage=%FinishMessage%
TargetName=%TargetName%
FriendlyName=%FriendlyName%
AppLaunched=%AppLaunched%
PostInstallCmd=%PostInstallCmd%
AdminQuietInstCmd=%AdminQuietInstCmd%
UserQuietInstCmd=%UserQuietInstCmd%
SourceFiles=SourceFiles
[SourceFiles]
SourceFiles0=C:\Windows\System32\
[SourceFiles0]
%FILE0%=


[Strings]
AppLaunched=subst.exe
PostInstallCmd=<None>
AdminQuietInstCmd=
UserQuietInstCmd=
FILE0="subst.exe"
DisplayLicense=
InstallPrompt=
```




