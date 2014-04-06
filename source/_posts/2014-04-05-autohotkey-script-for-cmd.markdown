---
layout: post
title: "Autohotkey script for cmd"
date: 2014-04-05 17:14:35 +0100
comments: true
categories:
---
Using windows without Autohotkey is using linux with no shell. No - I mean it.

For instance sooner or later you will use the cmd - this makes the xp bearable:

	;-------------------------------------------------------------------------------

	; Redefine only when the active window is a console window
	#IfWinActive ahk_class ConsoleWindowClass

	; Close Command Window with Ctrl+w
	$^w::
	WinGetTitle sTitle
	If (InStr(sTitle, "-")=0) {
			Send EXIT{Enter}
	} else {
			Send ^w
	}
	return

	; Ctrl+up / Down to scroll command window back and forward
	^Up::
	Send {WheelUp}
	return

	^Down::
	Send {WheelDown}
	return

	; Paste in command window
	^V::
	; Spanish menu (Editar->Pegar, I suppose English version is the same, Edit->Paste)
	Send !{Space}
	Send {up}{up}{up}{up}{right}{down}{down}{enter}
	return

	; Mark in command window
	^M::
	Send !{Space}
	Send {up}{up}{up}{up}{right}{enter}
	return

	; Select all in command window
	^A::
	Send !{Space}
	Send {up}{up}{up}{up}{right}{down}{down}{down}{enter}
	return

	#IfWinActive

	;-------------------------------------------------------------------------------


Some spanish guy holds the copyright I guess :D
