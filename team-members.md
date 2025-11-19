# ⭐ Create a Supervisor/Team Slicer for the Org Chart Visual

You can filter your Org Chart by team or supervisor by creating a separate **Supervisor/Team** table and using it as a slicer. Follow the steps below to set this up in your own Power BI report.

---

## Step 1 — Create the Supervisor/Team Table

Use the example report provided. It contains a Power Query script that automatically generates a Supervisor/Team table from your data.

1. Open the example report.  
2. Go to **Transform Data** → **Power Query Editor**.  
3. Open **Advanced Editor**.  
4. Copy the full Power Query code shown.

---

## Step 2 — Add the Code to Your Report

1. Open your own Power BI report.  
2. Go to **Transform Data** → **Power Query Editor**.  
3. Create a **New Query** → **Blank Query**.  
4. Open **Advanced Editor**, paste the code, and update the column names to match your dataset.  
5. Click **Close & Apply**.

---

## Step 3 — Create the Relationship

Connect the new Supervisor/Team table to your main dataset.

1. Go to **Model View**.  
2. Drag the **Team** or **Supervisor** column from the new table onto the matching column in your main table.  
3. Ensure the relationship is **one-to-many** (1:\*).

---

## Step 4 — Add the Slicer

1. Insert a **Slicer** visual onto your report page.  
2. Add the **Team** or **Supervisor** field from the new table into the slicer.  
3. (Optional) Format the slicer or apply additional filters as needed.

---

## Step 5 — Test the Filter

Select different supervisors or teams in the slicer.  
The Org Chart visual should now update dynamically based on your selection.

---

If you want, I can also add images, callouts, or a stylised layout for your documentation site.
