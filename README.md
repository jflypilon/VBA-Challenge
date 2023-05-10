'VBA-Challenge
'Module2 Homework
Sub Tickcount():
    
    For Each ws In Worksheets
        'i read as current row
        Dim i As Long
        'j read as new ticker identifier
        Dim j As Long
        'tickcount as new index counter for ticker row
        Dim Tickcount As Long
        'lastrow as long for column A
        Dim LastrowA As Long
        'lastrow as long for column I
        Dim LastrowI As Long
        'percentage change variable
        Dim PerChange As Double
        'Greatest Increase Over Year
        Dim Increase As Double
        'Greatest Decrease Over Year
        Dim Decrease As Double
        'Greatest Volume Over Year
        Dim Volume As Double
        
        'Call In Worksheets
        WorksheetsName = ws.Name
        
        'Name columns
        ws.Cells(1, 9).Value = "Ticker"
        ws.Cells(1, 10).Value = "Yearly Change"
        ws.Cells(1, 11).Value = "Percent Change"
        ws.Cells(1, 12).Value = "Total Stock Volume"
        
        'Additional Task Columns
        ws.Cells(1, 16).Value = "Ticker"
        ws.Cells(1, 17).Value = "Value"
        
        'Additional Table Modifiers
        ws.Cells(2, 15).Value = "Greatest Percent Increase"
        ws.Cells(3, 15).Value = "Greatest Percent Decrease"
        ws.Cells(4, 15).Value = "Greatest Total Volume"
        
        Tickcount = 2
        j = 2
        
        'Looking for last cell without data in columnA
        LastrowA = ws.Cells(Rows.Count, 1).End(xlUp).Row
        
            'Loop through all the rows
            For i = 2 To LastrowA
            
                'If the ticker name changed, write the ticker name in column 9
                If ws.Cells(i + 1, 1).Value <> ws.Cells(i, 1).Value Then
                ws.Cells(Tickcount, 9).Value = ws.Cells(i, 1).Value
                
                'If the ticker name changed, write the Yearly Change in column 10
                ws.Cells(Tickcount, 10).Value = Format(ws.Cells(i, 6).Value - ws.Cells(j, 3).Value, "Currency")
                                
                    'Conditional Formatting
                    If ws.Cells(Tickcount, 10).Value < 0 Then
                    ws.Cells(Tickcount, 10).Interior.ColorIndex = 9
                    
                    Else
                    ws.Cells(Tickcount, 10).Interior.ColorIndex = 4
                    
                    End If
                    
                    'Calculate % Change in Column 11
                    If ws.Cells(j, 3).Value <> 0 Then
                    PerChange = ((ws.Cells(i, 6).Value - ws.Cells(j, 3).Value) / ws.Cells(j, 3).Value)
                    
                    'Format as percentage
                    ws.Cells(Tickcount, 11).Value = Format(PerChange, "Percent")
                    
                    Else
                    ws.Cells(Tickcount, 11).Value = Format(0, "Percent")
                    
                    End If
                    
                    'Calculate Total Volume in Column 12
                    ws.Cells(Tickcount, 12).Value = WorksheetFunction.Sum(Range(ws.Cells(j, 7), ws.Cells(i, 7)))
                    
                    Tickcount = Tickcount + 1
                    
                    'Set new start row for ticker block
                    j = i + 1
                    
                    End If
                    
                    
                Next i
                
            'Looking for last cell without data in columnI
            LastrowI = ws.Cells(Rows.Count, 9).End(xlUp).Row
            
            'Prepare for summary
            Volume = ws.Cells(2, 12).Value
            Increase = ws.Cells(2, 11).Value
            Decrease = ws.Cells(2, 11).Value
            
                For i = 2 To LastrowI
            
                    ' Loop for summary
                    
                    'For greatest total volume check if next value is larger; if yes, take over a new value and populate ws.Cells
                    If ws.Cells(i, 12).Value > Volume Then
                    Volume = ws.Cells(i, 12).Value
                    ws.Cells(4, 16).Value = ws.Cells(i, 9).Value

                    Else

                    Volume = Volume
                
                    End If

                    'For greatest increase check if next value is larger; if yes, take over a new value and populate ws.Cells
                    If ws.Cells(i, 11).Value > Increase Then
                    Increase = ws.Cells(i, 11).Value
                    ws.Cells(2, 16).Value = ws.Cells(i, 9).Value

                    Else

                    Increase = Increase

                    End If

                    'For greatest decrease check if next value is smaller; if yes, take over a new value and populate ws.Cells
                    If ws.Cells(i, 11).Value < Decrease Then
                    Decrease = ws.Cells(i, 11).Value
                    ws.Cells(3, 16).Value = ws.Cells(i, 9).Value

                    Else

                    Decrease = Decrease
                       
                    End If
                    
                'Write Summary Results
                'Format Greatest Total Volume and Yearly Change as scientific
                ws.Cells(4, 17).Value = Format(Volume, "Scientific")
                ws.Cells(3, 17).Value = Format(Decrease, "Percent")

                'Format % Increase and Decrease As percent
                ws.Cells(2, 17).Value = Format(Increase, "Percent")
                        
                Next i
              
                'Adjust Column Width
                    
            Worksheets(WorksheetsName).Columns("A:Z").AutoFit
                
            
        Next ws
    
End Sub

