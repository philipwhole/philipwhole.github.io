---
layout: post
title:  "Macros For Automation"
date:   2023-11-17 00:00:00 -0000
categories: [blog]
---

# Module 1:  
  
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
    'Set sourceSheet = sourceWorkbook.Sheets("Sheet1")
    Set sourceSheet = sourceWorkbook.Sheets(1)

    '  Set the destination workbook
    Set destinationWorkbook = ThisWorkbook
   

    ' Set the destination sheet
    Set destinationSheet = destinationWorkbook.Sheets(1)
     

    ' Copy the data from the source sheet to the destination sheet
    sourceSheet.Copy Before:=destinationSheet

    ' Close the source workbook without saving changes
    sourceWorkbook.Close SaveChanges:=False
    
    destinationWorkbook.Sheets(1).Name = yyyy & mm

    End Sub

    Sub DeleteRedundantRows()

    'FindandRemovenewtransactions()
    'Find and Remove new transactions
    Dim col As String
    Dim lookup As String
    col = "C"
    lookup = "New Transactions"
 
    Dim rng As Range
    Set rng = ActiveSheet.Columns(col).find(What:=lookup, LookIn:=xlValues, LookAt:=xlWhole)
    
    If Not rng Is Nothing Then
        rng.Select
        DeleteRowsBelowSelectedCell
    End If
    
    'Find last row and delete it
    Dim lastRow As Long
    lastRow = FindLastRow(ActiveSheet, 1)
    ActiveSheet.Rows(lastRow).Delete
    
    End Sub
 

    Sub Main()
    
    Dim CurrentWorksheet As Worksheet
    Dim LookupWorksheet As Worksheet
    Dim lookupValue As Variant
    Dim lookupResult As Variant
    Dim i As Long
    Dim j As Variant
    Dim firstRow As Long
    Dim lastRow As Long
    Dim yyyy As String
    Dim mm As String
        
    yyyy = "2023" 'getInputYear
    mm = getInputMonth

    
    ' Set the Current worksheet
    Set CurrentWorksheet = ThisWorkbook.Sheets("banktransData")
    CurrentWorksheet.Activate
    
    ' Select Month
    SelectMonthData CInt(mm), CInt(yyyy) ' For the specified month and year

    'Calculate Column G
    CalculateColumnG
    
     
    ' Find the first row with data in Column A
     
    firstRow = FindSecondVisibleRow(CurrentWorksheet, 1)
    Debug.Print "firstRow: " & firstRow
    
    ' Find the last row with data in Column A
    lastRow = FindLastRow(CurrentWorksheet, 1)
    Debug.Print "lastRow: " & lastRow
     
    ' Set the lookup worksheet
    Set LookupWorksheet = ThisWorkbook.Sheets(1)
    
    
    'Loop to Call the CustomXLookup function
    
    For i = firstRow To lastRow
    lookupValue = CurrentWorksheet.Cells(i, 7).Value
     
    'Debug.Print lookupValue
    
    lookupResult = CustomXLookup(LookupWorksheet, lookupValue)
    CurrentWorksheet.Cells(i, 6).Value = lookupResult
    'Debug.Print lookupResult
    If lookupResult <> CustomXLookup(LookupWorksheet, lookupValue, -1) Then
    MsgBox "multiple results found"
    End If
    
     
    ' Use the MATCH function to find the position in Cash ledger
    j = Application.Match(lookupValue, LookupWorksheet.Range("Q:Q"), 0)

    
    ' Check if there was a match before attempting to delete the row
    If Not IsError(j) Then
        LookupWorksheet.Cells(j, 20).Value = CurrentWorksheet.Cells(i, 1).Value
    End If
    
    '
    Next i
     
    
    ' Add column F title in Bank statement
    CurrentWorksheet.Cells(1, 6).Value = "Trans"
    
    'Clear column G
    Columns(7).Delete
    
    ' Add column T title in Cash ledger
    LookupWorksheet.Cells(1, 20).Value = "Clear date" 
     
    End Sub

    Sub MainSub()
    Dim CurrentWorksheet As Worksheet
    Dim LookupWorksheet As Worksheet
    Dim lookupValue As Variant
    Dim lookupResult As Variant
    Dim i As Long
    Dim j As Variant
    Dim firstRow As Long
    Dim lastRow As Long
    Dim yyyy As String
    Dim mm As String
        
    yyyy = "2023" 'getInputYear
    mm = getInputMonth

    
    ' Set the Current worksheet
    Set CurrentWorksheet = ThisWorkbook.Sheets("banktransData")
    CurrentWorksheet.Activate
    
    ' Select Month
    SelectMonthData CInt(mm), CInt(yyyy) ' For the specified month and year

    'Calculate Column G
    CalculateColumnG
    
     
    ' Find the first row with data in Column A
     
    firstRow = FindSecondVisibleRow(CurrentWorksheet, 1)
    Debug.Print "firstRow: " & firstRow
    
    ' Find the last row with data in Column A
    lastRow = FindLastRow(CurrentWorksheet, 1)
    Debug.Print "lastRow: " & lastRow
     
    ' Set the lookup worksheet
    Set LookupWorksheet = ThisWorkbook.Sheets(1)
    
    
    'Loop to Call the CustomXLookup function
    
    For i = firstRow To lastRow
    lookupValue = CurrentWorksheet.Cells(i, 7).Value
     
    'Debug.Print lookupValue
    
    lookupResult = CustomXLookup(LookupWorksheet, lookupValue)
    CurrentWorksheet.Cells(i, 6).Value = lookupResult
    'Debug.Print lookupResult
     
    ' Use the MATCH function to find the position in Cash ledger
    j = Application.Match(lookupValue, LookupWorksheet.Range("Q:Q"), 0)
    Debug.Print lookupValue
    Debug.Print j
    
    ' Check if there was a match before attempting to delete the row
    If Not IsError(j) Then
        'LookupWorksheet.Rows(j).Delete
        LookupWorksheet.Cells(j, 20).Value = CurrentWorksheet.Cells(i, 1).Value
    End If
    
    '
    Next i
     
    
    ' Add column F title in Bank statement
    CurrentWorksheet.Cells(1, 6).Value = "Trans"
    
    'Clear column G
    Columns(7).Delete
    
    ' Add column T title in Cash ledger
    LookupWorksheet.Cells(1, 20).Value = "Clear date"
    
    'Delete first Sheet
    'ThisWorkbook.Sheets(1).Delete 
    End Sub 
    
# Module 2: 
        Function getInputYear() As String
    
        getInputYear = InputBox("Enter the year (e.g., 2022):")
        End Function

        Function getInputMonth() As String
            
        getInputMonth = InputBox("Enter the month (e.g., 05 for May):")
        End Function

# Module 3 
     
    Sub SelectMonthData(month As Integer, year As Integer)
    Columns("A:G").Select
    Selection.AutoFilter
    Dim lastDayOfMonth As Date
    lastDayOfMonth = DateSerial(year, month + 1, 0)
    ActiveSheet.Range("$A$1:$G$581").AutoFilter Field:=1, Operator:= _
        xlFilterValues, Criteria2:=Array(1, Format(lastDayOfMonth, "m/d/yyyy"))
    ActiveWindow.SmallScroll Down:=3
    End Sub

    Sub CalculateColumnG()
    Dim ws As Worksheet
    Dim lastRow As Long
    Dim i As Long
    
    ' Set the worksheet where your data is located
    Set ws = ThisWorkbook.Sheets("banktransData") ' Change to your sheet name
    
    ' Determine the last row with data (assuming data starts from row 1)
    lastRow = ws.Cells(ws.Rows.Count, "A").End(xlUp).Row
    Debug.Print lastRow
    
    
    ' Loop through each row
    For i = 2 To lastRow
        ' Calculate Column G (assuming Column D and Column C are numeric)
        ws.Cells(i, "G").Value = ws.Cells(i, "D").Value - ws.Cells(i, "C").Value
        Next i
    End Sub

# Module 4
        Function FindSecondVisibleRow(ws As Worksheet, col As Long) As Long
        
        Dim rng As Range
        Dim visibleCount As Long
        Dim i As Long
        
        ' Set the range to search for visible rows (adjust column as needed)
        Set rng = ws.Columns(col) ' Change to the appropriate column
        
        ' Initialize visible row count
        visibleCount = 0
        
        ' Loop through each row and find the second visible row
        For i = 1 To rng.Rows.Count
            If Not rng.Rows(i).Hidden Then
                visibleCount = visibleCount + 1
                If visibleCount = 2 Then
                    ' Return the row number of the second visible row
                    FindSecondVisibleRow = i
                    Exit Function
                End If
            End If
        Next i
        
        ' If the second visible row is not found, return -1
        FindSecondVisibleRow = -1
    End Function


    Function FindLastRow(ws As Worksheet, col As Long) As Long
        FindLastRow = ws.Cells(ws.Rows.Count, col).End(xlUp).Row
    End Function


    Sub DeleteRowsBelowSelectedCell()
    Dim selectedCell As Range
    Set selectedCell = Selection

    If Not selectedCell Is Nothing Then
        Dim lastRow As Long
        lastRow = Cells(Rows.Count, selectedCell.Column).End(xlUp).Row

        If lastRow > selectedCell.Row Then
            Rows(selectedCell.Row & ":" & lastRow).Delete
        Else
            MsgBox "No rows below the selected cell to delete."
        End If
    Else
        MsgBox "Please select a cell before running this macro."
    End If
End Sub


    Function CustomIndexMatch(TargetWorksheet As Worksheet, lookupValue As Variant) As Variant
    Dim matchRange As Range
    Dim resultRange As Range
    Dim matchValue As Variant
    
    ' Set the range to search for the lookupValue
    Set matchRange = TargetWorksheet.Range("Q:Q")
    
    ' Set the range where you want to return the result
    Set resultRange = TargetWorksheet.Range("E:E")
    
    ' Use the MATCH function to find the position
    matchValue = Application.Match(lookupValue, matchRange, 0)
    
    ' If matchValue is an error
    If IsError(matchValue) Then
        matchValue = Application.Match(lookupValue + 3, matchRange, 0)
    End If
    
    ' Use the INDEX function to return the value
    If Not IsError(matchValue) Then
        CustomIndexMatch = Application.Index(resultRange, matchValue)
    Else
        CustomIndexMatch = CVErr(xlErrNA) ' Return #N/A if match not found
    End If
End Function

    Function CustomXLookup(TargetWorksheet As Worksheet, lookupValue As Variant, Optional lookup_mode As Integer = 0) As Variant
    Dim lookupRange As Range
    Dim returnRange As Range
    Dim result As Variant
    
    ' Set the range to search for the lookupValue
    Set lookupRange = TargetWorksheet.Range("Q:Q")

    
    ' Set the range where you want to return the result
    Set returnRange = TargetWorksheet.Range("E:E")
     
    
    ' Use the XLOOKUP function to find and return the value
    result = Application.XLookup(lookupValue, lookupRange, returnRange, xlErrNA, 0)
    
    ' If result is an error
    If IsError(result) Then
        result = Application.XLookup(lookupValue + 3, lookupRange, returnRange, xlErrNA, 0)
    End If
    
    ' Return the result
    If Not IsError(result) Then
        CustomXLookup = result
    Else
        CustomXLookup = CVErr(xlErrNA) ' Return #N/A if match not found
    End If
    End Function 