# AppleScripts

Here's are some Apple Scripts I created. Use at your own risks.

Feel free to suggest any improvements as I'm not expert, or add other useful ones.

- [Fake Typing](#fake-typing)
- [Get System Settings Panes IDs](#get-system-settings-panes-ids)
- [Notifications grouping off](scripts/notifications-grouping-off.scpt)

## Fake Typing

Useful when you want to create a demo video, and ensure there's no typo.

```applescript
set command to "ls -lsa"

tell application "iTerm" to activate # Remove or change

delay 2 # So you can move the mouse away or anything else

repeat with i from 1 to count of characters in command
    set letter to character i of command
    tell application "System Events" to keystroke letter
    delay (random number from 0.025 to 0.075)
end repeat

tell application "System Events" to keystroke return
```

[Compiled version](scripts/fake-typing.scpt).

## Get System Settings Panes IDs

Use it to find the ID of a specific panel on the left menu of System Settings so you can easily automate it in a script.

```applescript
tell application "System Settings"
    activate

    delay 0.5
    set the current pane to pane id "com.apple.Notifications-Settings.extension"

    tell every pane
        its id
    end tell
end tell
```

[Compiled version](scripts/get-system-settings-panes-ids.scpt).

## Notifications grouping off

Useful to automatically set the notifications grouping setting to off for a specific application.

```applescript
set appName to "Google Chrome" # set to the app name displayed in the Notifications application list
set delayTime to 0.5

tell application "System Settings"

    activate
    delay delayTime
    #my delayUntilApp("System Settings")

    set the current pane to pane id "com.apple.Notifications-Settings.extension"
    delay delayTime
    #my delayUntilWindow("Notifications")


    #do shell script "open x-apple.systempreferences:com.apple.Notifications-Settings.extension"
    #delay delayTime

    tell application "System Events"
        tell front window of (first application process whose frontmost is true)

            delay delayTime
            set theButton to (first button of group 2 of scroll area 1 of group 1 of group 2 of splitter group 1 of group 1 whose value of attribute "AXAttributedDescription" contains appName)

            click theButton
            delay delayTime

            click pop up button 2 of group 4 of scroll area 1 of group 1 of group 2 of splitter group 1 of group 1
            delay delayTime

            pick menu item "Off" of menu 1 of pop up button 2 of group 4 of scroll area 1 of group 1 of group 2 of splitter group 1 of group 1
            delay delayTime

            click button 1
        end tell
    end tell

end tell
```

[Compiled version](scripts/notifications-grouping-off.scpt).
