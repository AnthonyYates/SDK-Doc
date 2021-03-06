---
uid: Numericobject
title: Numeric object
---


The Numeric manager handles the parsing of strings containing numbers. The system handles various decimal separators and thousand-separators.

 

```vb
    set nummgr = createObject("SuperOffice.NumericMgr")

    msg = "testing numeric mgr" & chr(10)

    set resmgr = wscript.createobject("SuperOffice.ResMgr")
    resmgr.Initialize("c:\crm_main\debug")

    lcid = resmgr.GetLCIDForLanguageCode( "NO" )

    s = "123 123.10"
    msg = msg & "ParseStringToDouble " & s & "=" & nummgr.ParseStringToDouble(lcid, s) & chr(10)
    s = "123 123,10"
    msg = msg & "ParseStringToDouble " & s & "=" & nummgr.ParseStringToDouble(lcid, s) & chr(10)
    s = "123,123.10"
    msg = msg & "ParseStringToDouble " & s & "=" & nummgr.ParseStringToDouble(lcid, s) & chr(10)
    s = "123123.10"
    msg = msg & "ParseStringToDouble " & s & "=" & nummgr.ParseStringToDouble(lcid, s) & chr(10)
    s = "123123,50"
    msg = msg & "ParseStringToDouble " & s & "=" & nummgr.ParseStringToDouble(lcid, s) & chr(10)

    msg = msg & chr(10)

    f=1234567.89

    msg = msg & "FormatDoubleString " & f & "=" & nummgr.FormatDoubleString(lcid, f) & chr(10)

    msgbox msg
```
 

![](../../images/NumericMgr.png)
