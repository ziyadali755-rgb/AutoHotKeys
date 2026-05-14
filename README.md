# AutoHotKeys
; ============================================================================
; Ziyad Custom Keyboard - AutoHotkey v2
; File: C:\Users\Ziyad\Ziyad_Custom_Keyboard.ahk
; ============================================================================

#Requires AutoHotkey v2.0
#SingleInstance Force

; ============================================================================
; GLOBAL VARIABLES
; ============================================================================

global CustomKeyboardEnabled := true
global TextNavigationEnabled := false
global DEBUG_MODE := true

; ============================================================================
; MONITOR COORDINATES
; ============================================================================

global Mon1_CentreX  := -1280
global Mon1_CentreY  := 366
global Mon1_TopLeftX := -1920
global Mon1_TopLeftY := 6
global Mon1_Width    := 1280
global Mon1_Height   := 720

global Mon2_CentreX  := 960
global Mon2_CentreY  := 540
global Mon2_TopLeftX := 0
global Mon2_TopLeftY := 0
global Mon2_Width    := 1920
global Mon2_Height   := 1080

global Mon3_CentreX  := 2560
global Mon3_CentreY  := 33
global Mon3_TopLeftX := 1920
global Mon3_TopLeftY := -327
global Mon3_Width    := 1280
global Mon3_Height   := 720

; ============================================================================
; APP PATHS AND URLS
; ============================================================================

global ChromeExe     := 'C:\Program Files\Google\Chrome\Application\chrome.exe'
global ChromeProfile := '--profile-directory="Default"'
global PgAdminPath   := 'C:\ProgramData\Microsoft\Windows\Start Menu\Programs\PostgreSQL 18\pgAdmin 4.lnk'

global URL_Notion     := 'https://www.notion.so/'
global URL_ChatGPT    := 'https://chatgpt.com/'
global URL_Claude     := 'https://claude.ai/'
global URL_Perplexity := 'https://www.perplexity.ai/'
global URL_Calendar   := 'https://calendar.google.com/'
global URL_Gmail      := 'https://mail.google.com/mail/?view=cm&fs=1'
global URL_CapCut     := 'https://www.capcut.com/'
global URL_Canva      := 'https://www.canva.com/'

; ============================================================================
; HELPER FUNCTIONS
; ============================================================================

DebugMsg(message) {
    global DEBUG_MODE
    if (DEBUG_MODE) {
        ToolTip(message)
        SetTimer(() => ToolTip(), -1500)
    }
}

ReleaseModifiers() {
    Send("{Ctrl up}{Shift up}{Alt up}{LWin up}{RWin up}")
}

OpenInChrome(url) {
    global ChromeExe, ChromeProfile
    try {
        Run('"' . ChromeExe . '" ' . ChromeProfile . ' "' . url . '"')
        DebugMsg("Chrome app launched: " . url)
    } catch {
        DebugMsg("Chrome not found")
    }
}

NewChromeTab() {
    global ChromeExe, ChromeProfile
    try {
        Run('"' . ChromeExe . '" ' . ChromeProfile . ' --new-tab')
        DebugMsg("New Chrome tab")
    } catch {
        DebugMsg("Chrome not found")
    }
}

OpenChromeBrowser() {
    global ChromeExe, ChromeProfile
    try {
        Run('"' . ChromeExe . '" ' . ChromeProfile)
        DebugMsg("Chrome opened")
    } catch {
        DebugMsg("Chrome not found")
    }
}

ReopenChromeTab() {
    Send("^+t")
    DebugMsg("Reopen Chrome tab")
}

MoveMouseToMonitor(monitorNumber) {
    global Mon1_CentreX, Mon1_CentreY
    global Mon2_CentreX, Mon2_CentreY
    global Mon3_CentreX, Mon3_CentreY
    if (monitorNumber = 1) {
        MouseMove(Mon1_CentreX, Mon1_CentreY, 0)
        DebugMsg("Mouse -> Monitor 1")
    } else if (monitorNumber = 2) {
        MouseMove(Mon2_CentreX, Mon2_CentreY, 0)
        DebugMsg("Mouse -> Monitor 2")
    } else if (monitorNumber = 3) {
        MouseMove(Mon3_CentreX, Mon3_CentreY, 0)
        DebugMsg("Mouse -> Monitor 3")
    }
}

MoveWindowToMonitor(monitorNumber) {
    global Mon1_TopLeftX, Mon1_TopLeftY, Mon1_Width, Mon1_Height
    global Mon2_TopLeftX, Mon2_TopLeftY, Mon2_Width, Mon2_Height
    global Mon3_TopLeftX, Mon3_TopLeftY, Mon3_Width, Mon3_Height
    activeId := WinExist("A")
    if (!activeId)
        return
    try {
        if (monitorNumber = 1) {
            WinMove(Mon1_TopLeftX, Mon1_TopLeftY, Mon1_Width, Mon1_Height, "ahk_id " . activeId)
            MoveMouseToMonitor(1)
            DebugMsg("Window -> Monitor 1")
        } else if (monitorNumber = 2) {
            WinMove(Mon2_TopLeftX, Mon2_TopLeftY, Mon2_Width, Mon2_Height, "ahk_id " . activeId)
            MoveMouseToMonitor(2)
            DebugMsg("Window -> Monitor 2")
        } else if (monitorNumber = 3) {
            WinMove(Mon3_TopLeftX, Mon3_TopLeftY, Mon3_Width, Mon3_Height, "ahk_id " . activeId)
            MoveMouseToMonitor(3)
            DebugMsg("Window -> Monitor 3")
        }
    } catch {
        DebugMsg("Window move failed")
    }
}

CycleMouseLeft() {
    global Mon2_TopLeftX, Mon3_TopLeftX
    MouseGetPos(&mx, &my)
    if (mx >= Mon3_TopLeftX)
        MoveMouseToMonitor(2)
    else if (mx >= Mon2_TopLeftX)
        MoveMouseToMonitor(1)
    else
        MoveMouseToMonitor(3)
}

CycleMouseRight() {
    global Mon2_TopLeftX, Mon3_TopLeftX
    MouseGetPos(&mx, &my)
    if (mx >= Mon3_TopLeftX)
        MoveMouseToMonitor(1)
    else if (mx >= Mon2_TopLeftX)
        MoveMouseToMonitor(3)
    else
        MoveMouseToMonitor(2)
}

ToggleCustomKeyboard() {
    global CustomKeyboardEnabled
    CustomKeyboardEnabled := !CustomKeyboardEnabled
    if (CustomKeyboardEnabled) {
        ToolTip("Keyboard ON")
        DebugMsg("Custom Keyboard ON")
    } else {
        ReleaseModifiers()
        ToolTip("Keyboard OFF")
        DebugMsg("Custom Keyboard OFF")
    }
    SetTimer(() => ToolTip(), -1500)
}

ToggleTextNavigation() {
    global TextNavigationEnabled
    TextNavigationEnabled := !TextNavigationEnabled
    if (TextNavigationEnabled) {
        ToolTip("Text Navigation ON")
        DebugMsg("Text Navigation ON")
    } else {
        ToolTip("Text Navigation OFF")
        DebugMsg("Text Navigation OFF")
    }
    SetTimer(() => ToolTip(), -1500)
}

ShowMouseCoords() {
    MouseGetPos(&mx, &my)
    ToolTip("Mouse: " . mx . ", " . my)
    SetTimer(() => ToolTip(), -2500)
}

ShowDebugStatus() {
    global CustomKeyboardEnabled, TextNavigationEnabled
    MouseGetPos(&mx, &my)
    capsState := GetKeyState("CapsLock", "T") ? "ON" : "OFF"
    numState  := GetKeyState("NumLock", "T")  ? "ON" : "OFF"
    title := ""
    try {
        title := WinGetTitle("A")
    } catch {
        title := "(unknown)"
    }
    msg := "=== DEBUG STATUS ===`n`n"
    msg .= "CustomKeyboardEnabled: " . (CustomKeyboardEnabled ? "true" : "false") . "`n"
    msg .= "TextNavigationEnabled: " . (TextNavigationEnabled ? "true" : "false") . "`n"
    msg .= "CapsLock: " . capsState . "`n"
    msg .= "NumLock: " . numState . "`n"
    msg .= "Mouse: " . mx . ", " . my . "`n"
    msg .= "Active Window: " . title
    MsgBox(msg, "Debug Status")
}

EmergencyExit() {
    ReleaseModifiers()
    DebugMsg("Emergency exit")
    ExitApp()
}

OpenExplorer() {
    Run("explorer.exe")
    DebugMsg("File Explorer")
}

OpenVSCode() {
    try {
        Run("code")
        DebugMsg("VS Code")
        return
    } catch {
    }
    try {
        Run("code.cmd")
        DebugMsg("VS Code")
        return
    } catch {
        DebugMsg("VS Code not found")
    }
}

OpenPgAdmin() {
    global PgAdminPath
    try {
        Run('"' . PgAdminPath . '"')
        DebugMsg("pgAdmin launched")
    } catch {
        DebugMsg("pgAdmin not found")
    }
}

OpenTerminal() {
    try {
        Run("wt.exe")
        DebugMsg("Terminal")
        return
    } catch {
    }
    try {
        Run("cmd.exe")
        DebugMsg("Terminal (cmd)")
    } catch {
        DebugMsg("Terminal not found")
    }
}

OpenSettings() {
    Run("ms-settings:")
    DebugMsg("Settings")
}

WindowsSearch() {
    Send("#s")
    DebugMsg("Search")
}

OpenDownloads() {
    Run("shell:Downloads")
    DebugMsg("Downloads")
}

OpenStickyNotes() {
    try {
        Run("explorer.exe shell:AppsFolder\Microsoft.MicrosoftStickyNotes_8wekyb3d8bbwe!App")
        DebugMsg("Sticky Notes")
    } catch {
        DebugMsg("Sticky Notes not found")
    }
}

OpenWiFi() {
    Run("ms-settings:network-wifi")
    DebugMsg("WiFi Settings")
}

OpenBluetooth() {
    Run("ms-settings:bluetooth")
    DebugMsg("Bluetooth Settings")
}

OpenConnectedDevices() {
    Run("ms-settings:connecteddevices")
    DebugMsg("Connected Devices")
}

CloseActiveWindow() {
    Send("!{F4}")
    DebugMsg("Close window")
}

NewFile() {
    Send("^n")
    DebugMsg("New File")
}

NewFolder() {
    Send("^+n")
    DebugMsg("New Folder")
}

SelectAllDelete() {
    Send("^a")
    Sleep(50)
    Send("{Delete}")
    DebugMsg("Select All + Delete")
}

DoubleClickAction() {
    Click(2)
}

PasteText() {
    Send("^v")
}

SelectAllCopy() {
    Send("^a")
    Sleep(50)
    Send("^c")
}

UndoAction() {
    Send("^z")
}

RedoAction() {
    Send("^y")
}

ClipboardHistory() {
    Send("#v")
}

SaveFile() {
    Send("^s")
    DebugMsg("Save")
}

SaveAsFile() {
    Send("^+s")
    DebugMsg("Save As")
}

SmartSplit() {
    Send("#z")
    DebugMsg("Snap Layouts")
}

MaximizeWindow() {
    Send("#{Up}")
    DebugMsg("Maximise")
}

SwitchLanguage() {
    Send("#{Space}")
    DebugMsg("Switch Language")
}

CycleVirtualDesktop() {
    Send("^#{Right}")
    DebugMsg("Cycle Desktop")
}

OpenTaskView() {
    Send("#{Tab}")
    DebugMsg("Task View")
}

OpenTaskManager() {
    Send("^+{Esc}")
    DebugMsg("Task Manager")
}

AutoScreenshot() {
    Send("#+s")
    DebugMsg("Screenshot")
}

RestartMedia() {
    Send("{Media_Prev}")
    DebugMsg("Restart Media")
}

PausePlay() {
    Send("{Media_Play_Pause}")
}

VideoBack5() {
    Send("{Left}")
    DebugMsg("Video -5s")
}

VideoForward5() {
    Send("{Right}")
    DebugMsg("Video +5s")
}

; ============================================================================
; STARTUP NOTIFICATION
; ============================================================================

DebugMsg("Script started")

; ============================================================================
; ALWAYS-AVAILABLE HOTKEYS (OUTSIDE ALL #HotIf BLOCKS)
; ============================================================================

^!Esc::EmergencyExit()
^!z::ToggleCustomKeyboard()
^!t::ShowMouseCoords()
^!d::ShowDebugStatus()

; ============================================================================
; MAIN CUSTOM SHORTCUTS
; ============================================================================

#HotIf CustomKeyboardEnabled

^Numpad1::MoveMouseToMonitor(1)
^Numpad5::MoveMouseToMonitor(2)
^Numpad3::MoveMouseToMonitor(3)

^NumpadEnd::MoveMouseToMonitor(1)
^NumpadClear::MoveMouseToMonitor(2)
^NumpadPgDn::MoveMouseToMonitor(3)

>^1::MoveWindowToMonitor(1)
>^2::MoveWindowToMonitor(2)
>^3::MoveWindowToMonitor(3)

~LShift Up::
{
    if (A_PriorKey = "LShift")
        CycleMouseLeft()
}

~RShift Up::
{
    if (A_PriorKey = "RShift")
        CycleMouseRight()
}

Insert::NewFile()
^Insert::NewFolder()
Home::OpenExplorer()
PgUp::OpenVSCode()
PgDn::OpenPgAdmin()
End::CloseActiveWindow()
Delete::SelectAllDelete()
Esc::DoubleClickAction()

Up::PasteText()
Down::SelectAllCopy()
Left::UndoAction()
Right::RedoAction()
^Up::ClipboardHistory()

^-::SmartSplit()
^+=::MaximizeWindow()

F1::OpenChromeBrowser()
F2::Send("{Volume_Down}")
F3::Send("{Volume_Up}")
F4::Send("{Volume_Mute}")
F5::SwitchLanguage()
F6::OpenWiFi()
F7::OpenBluetooth()
F8::OpenConnectedDevices()
F9::OpenInChrome(URL_Gmail)
F10::CycleVirtualDesktop()
F11::ToggleTextNavigation()
F12::OpenTaskView()
^F12::OpenTaskManager()

PrintScreen::AutoScreenshot()
ScrollLock::RestartMedia()
Pause::PausePlay()
>^a::VideoBack5()
>^d::VideoForward5()

#HotIf

; ============================================================================
; NUMLOCK ON SECTION
; ============================================================================

#HotIf CustomKeyboardEnabled && GetKeyState("NumLock", "T")

Numpad1::Send("^c")
Numpad2::Send("^x")
Numpad3::NewChromeTab()
!Numpad3::ReopenChromeTab()
Numpad4::OpenInChrome(URL_Notion)
Numpad5::OpenInChrome(URL_ChatGPT)
Numpad6::OpenInChrome(URL_Claude)
Numpad7::OpenInChrome(URL_Perplexity)
Numpad9::OpenDownloads()
Numpad0::OpenStickyNotes()
NumpadDot::Send("{Delete}")
NumpadAdd::SaveFile()
^NumpadAdd::SaveAsFile()
NumpadDiv::OpenTerminal()
NumpadMult::OpenSettings()
NumpadSub::WindowsSearch()
NumpadEnter::OpenInChrome(URL_Calendar)

#HotIf

; ============================================================================
; NUMLOCK OFF SECTION
; ============================================================================

#HotIf CustomKeyboardEnabled && !GetKeyState("NumLock", "T")

NumpadEnd::OpenInChrome(URL_CapCut)
NumpadDown::OpenInChrome(URL_Canva)

#HotIf

; ============================================================================
; TEXT NAVIGATION SECTION (overrides main arrows when active)
; ============================================================================

#HotIf CustomKeyboardEnabled && TextNavigationEnabled && GetKeyState("CapsLock", "T") && GetKeyState("Space", "P") && GetKeyState("Ctrl", "P")
Up::Send("^+{Up}")
Down::Send("^+{Down}")
Left::Send("^+{Up}")
Right::Send("^+{Down}")
#HotIf

#HotIf CustomKeyboardEnabled && TextNavigationEnabled && GetKeyState("CapsLock", "T") && GetKeyState("Space", "P") && !GetKeyState("Ctrl", "P")
Up::Send("+{Up}")
Down::Send("+{Down}")
Left::Send("+{Home}")
Right::Send("+{End}")
#HotIf

#HotIf CustomKeyboardEnabled && TextNavigationEnabled && GetKeyState("CapsLock", "T") && GetKeyState("NumLock", "T") && !GetKeyState("Space", "P")
Up::Send("^+{Up}")
Down::Send("^+{Down}")
Left::Send("^+{Left}")
Right::Send("^+{Right}")
#HotIf

#HotIf CustomKeyboardEnabled && TextNavigationEnabled && GetKeyState("CapsLock", "T") && !GetKeyState("NumLock", "T") && !GetKeyState("Space", "P")
Up::Send("+{Up}")
Down::Send("+{Down}")
Left::Send("+{Left}")
Right::Send("+{Right}")
#HotIf
