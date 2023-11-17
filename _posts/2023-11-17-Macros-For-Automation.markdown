---
layout: post
title:  "Macros For Automation"
date:   2023-11-17 00:00:00 -0000
categories: [blog]
---

# Module 1: CopySourceFile Macro

```vba
Sub CopySourceFile()
    Dim yyyy As String
    Dim mm As String
    
    yyyy = "2023" 'getInputYear
    mm = getInputMonth

    Dim sourceWorkbook As Workbook
    Dim destinationWorkbook As Workbook
    Dim sourceSheet As Worksheet
    Dim destinationSheet As Worksheet

    ' Set the source workbook (file you are copying from)
    Debug.Print ThisWorkbook.Path
    
    Set sourceWorkbook = Workbooks.Open(ThisWorkbook.Path & "\" & yyyy & mm & ".xlsx")

    ' Set the source sheet
    ' Set sourceSheet = sourceWorkbook.Sheets("Sheet1")
    Set sourceSheet = sourceWorkbook.Sheets(1)

    ' Set the destination workbook
    Set destinationWorkbook = ThisWorkbook

    ' Set the destination sheet
    Set destinationSheet = destinationWorkbook.Sheets(1)

    ' Copy the data from the source sheet to the destination sheet
    sourceSheet.Copy Before:=destinationSheet

    ' Close the source workbook without saving changes
    sourceWorkbook.Close SaveChanges:=False
    
    ' Rename the destination sheet
    destinationWorkbook.Sheets(1).Name = yyyy & mm
End Sub


 