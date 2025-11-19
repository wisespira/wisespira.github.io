# ⭐ Create Reporting Lines (Path) Using Power Query (M)

This guide explains how to generate **reporting line paths** in Power BI using Power Query (M).  
The query builds a `Path` field based on `Employee ID` and `Manager ID`, which you can then use with the **Org Chart Drag & Drop** visual.

---

## Step 1 — Prepare Your Example Report & Data

You can either:

- Use the **example report** provided with the sample Excel file, or  
- Point the query to your own Excel file with similar columns.

The M code below expects an Excel file with at least these columns:

- `Employee ID`
- `Manager ID`
- `Name`

Other columns are removed later in the query.

If you’re using your own file, update the file path in the `File.Contents` step.

---

## Step 2 — Open Power Query and Create a Blank Query

1. Open your Power BI report.  
2. Go to **Home** → **Transform Data** to open **Power Query Editor**.  
3. In Power Query, go to **Home** → **New Source** → **Blank Query**.  
4. Right-click the new query in the Queries pane and select **Advanced Editor**.

---

## Step 3 — Paste the M Code

Replace any existing code in the **Advanced Editor** with the M code below.

> 🔁 **Important:**  
> - Update the file path in `File.Contents("C:\Users\irawa\Desktop\Structural Issues.xlsx")` to match your own Excel file location.  
> - Make sure the column names used in the query exist in your sheet, or remove/comment out any that don’t.
{% raw %}
```m
let
    Source = Excel.Workbook(
        File.Contents("C:\Users\irawa\Desktop\Structural Issues.xlsx"),
        null,
        true
    ),

    Sheet1_Sheet = Source{[Item = "Sheet1", Kind = "Sheet"]}[Data],

    #"Promoted Headers" =
        Table.PromoteHeaders(
            Sheet1_Sheet,
            [PromoteAllScalars = true]
        ),

    #"Changed Type" =
        Table.TransformColumnTypes(
            #"Promoted Headers",
            {
                {"Employee ID", Int64.Type},
                {"Manager ID", Int64.Type},
                {"Name", type text},
                {"Job Title", type text},
                {"Department", type text},
                {"Team", type text},
                {"Country", type text},
                {"Contract Type", type text},
                {"Seniority Level", type text},
                {"Department Color", type text},
                {"Structural Issues", type any},
                {"Comment", type text},
                {"Salary", Int64.Type},
                {"Budget", type text},
                {"Budget Colour", type text}
            }
        ),

    #"Removed Columns" =
        Table.RemoveColumns(
            #"Changed Type",
            {
                "Job Title",
                "Department",
                "Team",
                "Country",
                "Contract Type",
                "Seniority Level",
                "Department Color",
                "Structural Issues",
                "Comment",
                "Salary",
                "Budget",
                "Budget Colour"
                // Only include these if they actually exist in your sheet:
                // "dottedline",
                // "Budget 2"
            }
        ),

    Base = #"Removed Columns",

    // Add Path column WITHOUT a separate BuildPath function
    #"Added Path" =
        Table.AddColumn(
            Base,
            "Path",
            each
                let
                    startId = [Employee ID],

                    // If there's no Employee ID, no path
                    pathList =
                        if startId = null then
                            {}
                        else
                            List.Generate(
                                // Initial state: start at this employee
                                () => [Current = startId],

                                // Keep going while Current is not null
                                each [Current] <> null,

                                // Next state: move to the manager of Current
                                each
                                    let
                                        row =
                                            Table.SelectRows(
                                                Base,
                                                (r) => r[Employee ID] = [Current]
                                            ),
                                        manager =
                                            if Table.RowCount(row) = 0 then
                                                null
                                            else
                                                row{0}[Manager ID],

                                        // Stop if manager is null or self-managed
                                        next =
                                            if manager = [Current] then
                                                null
                                            else
                                                manager
                                    in
                                        [Current = next],

                                // What to collect each iteration:
                                each [Current]
                            ),

                    // Build text: top manager > ... > employee
                    pathText =
                        if List.IsEmpty(pathList) then
                            null
                        else
                            Text.Combine(
                                List.Transform(
                                    List.Reverse(pathList),
                                    each Text.From(_)
                                ),
                                " > "
                            )
                in
                    pathText,
            type text
        ),

    // Optional: remove extra columns if they exist in your data
    #"Removed Columns1" =
        try
            Table.RemoveColumns(#"Added Path", {"dottedline", "Budget 2"})
        otherwise
            #"Added Path",

    #"Split Column by Delimiter" =
        Table.ExpandListColumn(
            Table.TransformColumns(
                #"Removed Columns1",
                {
                    {
                        "Path",
                        Splitter.SplitTextByDelimiter(" > ", QuoteStyle.None),
                        let
                            itemType = (type nullable text) meta [Serialized.Text = true]
                        in
                            type {itemType}
                    }
                }
            ),
            "Path"
        ),

    #"Changed Type1" =
        Table.TransformColumnTypes(
            #"Split Column by Delimiter",
            {{"Path", Int64.Type}}
        ),

    #"Removed Columns2" =
        Table.RemoveColumns(
            #"Changed Type1",
            {"Employee ID"}
        ),

    #"Renamed Columns" =
        Table.RenameColumns(
            #"Removed Columns2",
            {{"Path", "Employee ID"}}
        ),

    #"Changed Type2" =
        Table.TransformColumnTypes(
            #"Renamed Columns",
            {{"Employee ID", type text}, {"Manager ID", type text}}
        )
in
    #"Changed Type2"
```
{% endraw %}

Click Done, then Close & Apply to load the query.

Step 4 — Use the Output Table in Your Org Chart Visual

The final query (#"Changed Type2") returns a table where:

Each row represents part of a reporting path.

Employee ID and Manager ID are converted to text, which is required for many visuals.

The expanded Path logic creates multiple rows to represent the full chain from top manager down.

You can:

Use this new query as your main dataset for the Org Chart visual, or

Join/relate it to your existing headcount table in Model View using Employee ID and Manager ID.

Step 5 — Validate the Reporting Lines

Load the table into Data View.

Sort or filter by Employee ID and Manager ID to confirm the hierarchy looks correct.

Add the table to the Org Chart Drag & Drop visual and check that the reporting lines match your organisation structure.

If you’d like, I can also merge this guide with your existing documentation pages or add image placeholders (e.g. screenshots of the Advanced Editor, Data View, or Org Chart visual).
