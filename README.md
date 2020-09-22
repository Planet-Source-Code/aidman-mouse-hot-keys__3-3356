<div align="center">

## Mouse Hot Keys


</div>

### Description

This code was or original made in Visual Basic, you can find it there too, but I rewrote the code to Visual C++ for better performance, VB is no match for C++! I made the program for my Pentium 90 MHz that had no mouse, because it needed serial port mouse which I didn’t have. So hope fully you will also have use for this code or you may learn from it, because it is an easy, very fast and well explained program. It let’s you control your mouse courser actions by custom keyboard keys and custom movements speed. Showing the easiest way of hotkeys and controlling your mouse cursor movements and button actions. It has been tested and works in all Windows platforms (95, 98, ME, NT, 2000, XP) and all games (Half-life, Counter-Strike, Quake III arena). There are many functions including: * Moving at all directions * Left, right and middle button press * Single clicking * Double clicking * Drag and drop * Several keys up and down at once. All in just one single small and very fast procedur, the Main procedur. Whit out anything else like form, dll or ocx. Read "Assumes" in the Source Code for using instructions and notes, I will gladly accept any comments, this is my first Program that I have published in C++, enjoy =)
 
### More Info
 
All Keys are Numpad Keys, Numlock must be toggled. When using the program in games, increase the MoveSpeed constant.


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Aidman](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/aidman.md)
**Level**          |Intermediate
**User Rating**    |4.8 (24 globes from 5 users)
**Compatibility**  |C\+\+ \(general\), Microsoft Visual C\+\+, Borland C\+\+
**Category**       |[Miscellaneous](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/miscellaneous__3-1.md)
**World**          |[C / C\+\+](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/c-c.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/aidman-mouse-hot-keys__3-3356/archive/master.zip)

### API Declarations

```
#include <windows.h> // Windows header file
```


### Source Code

```
const int MoveSpeed = 5; // Move Mouse speed in pixels.
const int MoveUp = 0; // action number for move Mouse up.
const int MoveDown = 1; // action number for move Mouse down.
const int MoveLeft = 2; // action number for move Mouse left.
const int MoveRight = 3; // action number for move Mouse right.
const int ButtonLeft = 4; // action number for left Mouse button.
const int ButtonMiddle = 5; // action number for middle Mouse button.
const int ButtonRight = 6; // action number for right Mouse button.
const int EndProgram = 7; // action number for end Program.
int WINAPI WinMain(HINSTANCE hThisInstance, HINSTANCE hPrevInstance, LPSTR lpszArgument, int nFunsterStil) { // Main procedur.
	MSG Msg; // Message from Windows.
	POINT MousePos; // Windows Mouse position.
  int KeyCode[7]; // Key numbers for all actions.
  bool KeyState[7]; // Key press values for action.
  bool OldKeyState[7]; // Old Key press values for move actions only.
  bool MoveKeys = false; // Value of any pressed move action.
	bool EndLoop = false; // Value for continuation of Do loop.
  int Count; // For loop counter.
	// Remember Numlock key must be toggled.
  KeyCode[MoveUp] = VK_NUMPAD8; // Use Numpad Key 8 as move Mouse up action.
  KeyCode[MoveDown] = VK_NUMPAD5; // Use Numpad Key 5 as move Mouse down action.
  KeyCode[MoveLeft] = VK_NUMPAD4; // Use Numpad Key 4 as move Mouse left action.
  KeyCode[MoveRight] = VK_NUMPAD6; // Use Numpad Key 6 as move Mouse right action.
  KeyCode[ButtonLeft] = VK_NUMPAD7; // Use Numpad Key 7 as left Mouse button action.
  KeyCode[ButtonMiddle] = VK_NUMPAD2; // Use Numpad Key 2 as middle Mouse button action.
  KeyCode[ButtonRight] = VK_NUMPAD9; // Use Numpad Key 9 as right Mouse button action.
  KeyCode[EndProgram] = VK_DECIMAL; // Use Numpad Key "." as end Program action.
  while (EndLoop == false) { // The Program Do loop.
    Sleep(1); // Pause program one millisecond.
    PeekMessage(&Msg, 0, 0, 0, PM_REMOVE); // Get message from Windows.
    switch (Msg.message) { // Message Value.
      case WM_CLOSE: // Abort Program.
      case WM_QUIT: // Exit Program.
      case WM_DESTROY: // Terminate Program.
				EndLoop = true; // End Program loop.
    }
    TranslateMessage(&Msg); // Convert Message.
    DispatchMessage(&Msg); // End Message.
    for (Count = MoveUp; Count <= EndProgram; Count++) { // For loop for Key press.
      GetAsyncKeyState(KeyCode[Count]); // Clear last Key press.
      if (Count >= ButtonLeft) { OldKeyState[Count] = KeyState[Count]; } // Old Key press for move actions.
      KeyState[Count] = false; // Clear last Key press.
      if (GetAsyncKeyState(KeyCode[Count]) != 0) { // Get current Key press.
        KeyState[Count] = true; // Key is pressed.
        if (Count <= MoveRight) { MoveKeys = true; } // Move action key is pressed.
      }
    }
    if (KeyState[EndProgram] == true) { EndLoop = true; } // End Program loop if end key is pressed.
		if (MoveKeys == true) { // Do following if any move keys are pressed.
      GetCursorPos(&MousePos); // Get current Mouse position.
      for (Count = MoveUp; Count <= MoveRight; Count++) { // For loop for move actions.
        if (KeyState[Count] == true) { // Do following if move key is pressed.
          switch (Count) { // Choose Mouse position modification action.
						case MoveUp: MousePos.y = MousePos.y - MoveSpeed; break; // Move Mouse up action.
            case MoveDown: MousePos.y = MousePos.y + MoveSpeed; break; // Move Mouse down action.
            case MoveLeft: MousePos.x = MousePos.x - MoveSpeed; break; // Move Mouse left action.
            case MoveRight: MousePos.x = MousePos.x + MoveSpeed; // Move Mouse right action.
          }
        }
      }
      SetCursorPos(MousePos.x, MousePos.y); // Update Mouse position.
      mouse_event(MOUSEEVENTF_MOVE, 0, 0, 0, 0); // Inform Windows of Mouse move action.
    }
    for (Count = ButtonLeft; Count <= ButtonRight; Count++) { // For loop for Mouse button.
      if ((KeyState[Count] == true) && (OldKeyState[Count] == false)) { // Do following if Key button is down.
        switch (Count) { // Choose Mouse button action for key button down.
          case ButtonLeft: mouse_event(MOUSEEVENTF_LEFTDOWN, 0, 0, 0, 0); break; // Left Mouse button down.
          case ButtonMiddle: mouse_event(MOUSEEVENTF_MIDDLEDOWN, 0, 0, 0, 0); break; // Middle Mouse button down.
          case ButtonRight: mouse_event(MOUSEEVENTF_RIGHTDOWN, 0, 0, 0, 0); // Right Mouse button down.
        }
      } else if ((KeyState[Count] == false) && (OldKeyState[Count] == true)) { // Do following if Key button is up.
        switch (Count) { // Choose Mouse button action for key button up.
          case ButtonLeft: mouse_event(MOUSEEVENTF_LEFTUP, 0, 0, 0, 0); break; // Left Mouse button up.
          case ButtonMiddle: mouse_event(MOUSEEVENTF_MIDDLEUP, 0, 0, 0, 0); break; // Middle Mouse button up.
          case ButtonRight: mouse_event(MOUSEEVENTF_RIGHTUP, 0, 0, 0, 0); // Right Mouse button up.
        }
      }
    }
  }
  Beep(400, 100); // Inform user that Program has ended with sound beep.
  return Msg.wParam; // Return reson for Program end.
}
```

