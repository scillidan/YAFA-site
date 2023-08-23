
### Reference

1. https://blog.danskingdom.com/Change-the-default-AutoHotkey-script-editor/

## KeyMap

In `.sublime-snippet`

```
<snippet>
  <content><![CDATA[
<i class="far fa-circle"></i>
]]></content>
  <tabTrigger>ic1</tabTrigger>
  <description>far fa-circle</description>
</snippet>
```

```
<snippet>
  <content><![CDATA[
⚪
]]></content>
  <tabTrigger>c1</tabTrigger>
  <description></description>
</snippet>
```

In `KeyMap.ahk`

```
::\c1::⚪
::\c2::⚫
```

In `my-cheat-sheet.d`

```
:-      | :-
:-       | :-
`abbr`    | abbreviation
`abt`     | about
`ack`     | acknowledgement
`ahk`     | autohotkey
`analyt`  | analytic
`anlz`    | analyzer
`annot`   | annotation
`arc`     | archive
`ase`     | aseprite
`aud`     | audio
`aut`     | author
`av`      | antivirus
`bak`     | backup
`bkm`     | bookmark
`bksp`    | backspace
`bot`     | robot
`brows`   | browser
``` 

In `AHKExpansionsList.ahk`

```
::abbr::abbreviation
::abt::about
::ack::acknowledgement
::ahk::autohotkey
::analyt::analytic
::anlz::analyzer
::annot::annotation
::arc::archive
::ase::aseprite
::aud::audio
::aut::author
::av::antivirus
::bak::backup
::bkm::bookmark
::bksp::backspace
::bot::robot
::brows::browser
```

## AHKExpansionsList

### Reference

1. https://www.autohotkey.com/boards/viewtopic.php?t=104032